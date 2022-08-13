# Class RandomAccessFile

可实现文件读写随机访问。

```java
public class UsingRandomAccessFile {
    static String file = "rtest.dat";

    public static void display() {
        try (RandomAccessFile rf = new RandomAccessFile(file, "r")) {
            for (int i = 0; i < 8; i++) {
                System.out.println("Value " + i + ": " + rf.readDouble());
            }
            //System.out.println(rf.readUTF());
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) {
        try (RandomAccessFile rf = new RandomAccessFile(file, "rw")) {
            for (int i = 0; i < 7; i++) {
                // double 8 字节
                rf.writeDouble(i * 1.414);
            }
            //rf.writeUTF("The end of the file");
            rf.close();
            //display();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        try (RandomAccessFile rf = new RandomAccessFile(file, "rw")) {
            // 移动到文件末尾后，继续写可以拓展文件
            rf.seek(7 * 8);
            rf.writeDouble(47.0001);
            rf.close();
            display();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```