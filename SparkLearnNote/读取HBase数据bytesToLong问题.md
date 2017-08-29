# 读取HBase数据时对Long类型的时间处理问题

    public static long bytesToLong(byte[] bytes) {
            long l = 1l;
            if (bytes.length > 8) {
                System.out.println(bytes.length);
                l = Long.parseLong(new String(bytes));
            } else {
                ByteBuffer buffer = ByteBuffer.allocate(bytes.length);
                buffer.put(bytes, 0, bytes.length);
                buffer.flip();// need flip
                l = buffer.getLong();
            }
    
            return l;
        }