---
date created: 2023-03-19, 10:22:08
date modified: 2023-03-19, 10:22:16
---

# Meta

- alias:
- parent ::
- siblings ::
- child ::
- refs:
    - https://stackoverflow.com/questions/13003871/how-do-i-get-the-instance-of-sun-misc-unsafe
    - AQS 中的代码

---

```java
public class ReflectTest {
    public static void main(String[] args) {
        Student student = new Student();
        student.setState(123);

        System.out.println(student.compareAndSetState(123, 99));
        System.out.println(student.getState()); // 99
    }
}

class Student {
    private int state;

    private static final long stateOffset;

    private static final Unsafe unsafe;

    static {
        try {
            Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
            theUnsafe.setAccessible(true);
            unsafe = (Unsafe) theUnsafe.get(null);
            stateOffset = unsafe.objectFieldOffset(Student.class.getDeclaredField("state"));
        } catch (Exception ex) {
            throw new Error(ex);
        }
    }

    public void setState(int state) {
        this.state = state;
    }

    public int getState() {
        return state;
    }

    public final boolean compareAndSetState(int expected, int update) {
        return unsafe.compareAndSwapInt(this, stateOffset, expected, update);
    }
}
```