# 在Win10下启动tensorboard

    d:
    cd D:\ProgramFiles\Anaconda3\Lib\site-packages\tensorflow\tensorboard
    python tensorboard.py --logdir=D:\Workspace\Tensorflow\mnist_with_summaries\logs

直接使用tensorboard --logdir=logs的时候在Mac下能够正常运行，但是在Win下则会报错：
tensorboard Fatal error in launcher: Unable to create process using '"'


    d:
    cd D:\Workspace\Tensorflow\tensorboard
    python .\full_code.py
    python D:\ProgramFiles\Anaconda3\Lib\site-packages\tensorflow\tensorboard\tensor
    board.py --logdir=logs