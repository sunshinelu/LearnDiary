# spark 中文编码处理

> 参考链接
>
> spark 中文编码处理
> <https://www.cnblogs.com/westfly/p/spark_encoding_convert.html>
>

日志的格式是GBK编码的，而hadoop上的编码是用UTF-8写死的，导致最终输出乱码。

研究了下Java的编码问题。

网上其实对spark输入文件是GBK编码有现成的解决方案，具体代码如下

    import org.apache.hadoop.io.LongWritable
    import org.apache.hadoop.io.Text
    import org.apache.hadoop.mapred.TextInputFormat
    
    rdd = ctx.hadoopFile(file_list, classOf[TextInputFormat],
                classOf[LongWritable], classOf[Text]).map(
                pair => new String(pair._2.getBytes, 0, pair._2.getLength, "GBK"))


这种想法的来源是基于


    public static Text transformTextToUTF8(Text text, String encoding) {
        String value = null;
        try {
        value = new String(text.getBytes(), 0, text.getLength(), encoding);
        } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
        }
        return new Text(value);
    }


但这种方法还有一个问题，

大家都知道gbk是2~3个字节编码的。如果日志中按照直接截断，导致按照gbk读取文件的时候，将后面的分隔符\t一并读取了 ，导致按照\t split的时候，字段的个数不对（或者说顺序错位了）。

这个时候，需要找到一种单字节的解析方案，即 ISO-8859-1编码。代码如下

    rdd = ctx.hadoopFile(file_list, classOf[TextInputFormat],
                classOf[LongWritable], classOf[Text]).map(
                pair => new String(pair._2.getBytes, 0, pair._2.getLength, "ISO-8859-1"))


但这又带来了一个问题，即输出的结果（按照UTF-8存储）是乱码，不可用。

 
如果我们换一种思路来考虑这个问题，Java或scala中如何将一个gbk文件转换为UTF8？网上有很多的现成的代码，具体到我们的场景，以行为单位处理的话，示例代码如下


    public class Encoding {
        private static String kISOEncoding = "ISO-8859-1";
        private static String kGBKEncoding = "GBK";
        private static String kUTF8Encoding = "UTF-8";
        
        public static void main(String[] args) throws UnsupportedEncodingException {
            try {
                File out_file = new File(args[1]);
                Writer out = new BufferedWriter(new OutputStreamWriter(
                             new FileOutputStream(out_file), kUTF8Encoding));
                List<String> lines = Files.readAllLines(Paths.get(args[0]), Charset.forName(kGBKEncoding));
                for (String line : lines) {
                    out.append(line).append("\n");
                }
                out.flush();
                out.close();
            } catch (IOException e) {
                System.out.println(e);
            }
        }
    }



如上的代码给了我们一个启示，即在写入文件的时候，系统自动进行了编码的转换，我们没必要对行进行单独的直接转换处理。

通过查询资料，Java中字符编码是内部编码，即字节流按照编码转化为String。

所谓结合以上两点认识，我们模拟在spark上以ISO-8859-1

打开文件和以UTF-8写入文件的过程，发现只需要将其强制转换为GBK的string即可，最终得到的文件以UTF-8打开不是乱码，具体代码如下。

    
    public class Encoding {
        private static String kISOEncoding = "ISO-8859-1";
        private static String kGBKEncoding = "GBK";
        private static String kUTF8Encoding = "UTF-8";
        
        public static void main(String[] args) throws UnsupportedEncodingException {
            try {
                File out_file = new File(args[1]);
                Writer out = new BufferedWriter(new OutputStreamWriter(
                             new FileOutputStream(out_file), kUTF8Encoding));
                List<String> lines = Files.readAllLines(Paths.get(args[0]), Charset.forName(kISOEncoding));
                for (String line : lines) {
                    String gbk_str = new String(line.getBytes(kISOEncoding), kGBKEncoding);
                    out.append(gbk_str).append("\n");
                }
                out.flush();
                out.close();
            } catch (IOException e) {
                System.out.println(e);
            }
        }
    }
