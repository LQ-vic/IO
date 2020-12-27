# IOpublic class Student {
    //姓名
    private String name;
    //语文成绩
    private int chinese;
    //数学成绩
    private int math;
    //英语成绩
    private int english;
    public Student(){
        super();
    }
    public Student(String name,int chinese,int math,int english){
        super();
        this.name = name;
        this.chinese = chinese;
        this.math = math;
        this.english = english;
    }
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
    public int getChinese(){
        return chinese;
    }
    public void setChinese(int chinese){
        this.chinese = chinese;
    }
    public int getMath(){
        return math;
    }
    public void setMath(int math){
        this.math = math;
    }
    public int getEnglish(){
        return english;
    }
    public void setEnglish(int english){
        this.english = english;
    }
    public int getSum(){
        return this.chinese + this.math + this.english;
    }

}











import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Comparator;
import java.util.Scanner;
import java.util.TreeSet;

public class TreeSetToFileDemo {
    public static void main(String[] args) throws IOException {
        TreeSet<Student> ts = new TreeSet<Student>(new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
               int num = s2.getSum()-s1.getSum();
               int num2 = num == 0 ?s1.getChinese()-s2.getChinese():num;
               int num3 = num2 == 0?s1.getMath()-s2.getMath():num2;
               int num4 = num3 == 0?s1.getName().compareTo(s2.getName()):num3;
               return num4;
            }
        });
        for (int i = 0;i < 5;i++){
            Scanner sc = new Scanner(System.in);
            System.out.println("请录入第"+(i+ 1)+"个学生信息：");
            System.out.println("姓名:");
            String name = sc.nextLine();
            System.out.println("语文成绩:");
            int chinese = sc.nextInt();
            System.out.println("数学成绩:");
            int math = sc.nextInt();
            System.out.println("英语成绩:");
            int english = sc.nextInt();

            Student s = new Student();
            s.setName(name);
            s.setChinese(chinese);
            s.setMath(math);
            s.setEnglish(english);

            ts.add(s);
        }

        BufferedWriter bw = new BufferedWriter(new FileWriter("Packday19-IO\\ts.txt"));

        for (Student s : ts){

            StringBuilder sb = new StringBuilder();
            sb.append(s.getName()).append(",").append(s.getChinese()).append(",").append(s.getMath()).append(",").append(s.getEnglish()).append(",").append(s.getSum());
            bw.write(sb.toString());
            bw.newLine();
            bw.flush();
        }
        bw.close();
    }
}










package itheima_08;

import java.io.*;

public class CopyFolderDemo {

        public static void main(String[] args) throws IOException {
            //创建数据源目录File对象，路径是E:\\itcast
            File srcFolder = new File("/Users/regardless/Desktop/itcast");

            //获取数据源目录File对象的名称(itcast)
            String srcFolderName = srcFolder.getName();

            //创建目的地目录File对象，路径名是模块名+itcast组成(/Users/regardless/Desktop/itcast)
            File destFolder = new File("Packday19-IO",srcFolderName);

            //判断目的地目录对应的File是否存在，如果不存在，就创建
            if(!destFolder.exists()) {
                destFolder.mkdir();
            }

            //获取数据源目录下所有文件的File数组
            File[] listFiles = srcFolder.listFiles();

            //遍历File数组，得到每一个File对象，该File对象，其实就是数据源文件
            for(File srcFile : listFiles) {
                //数据源文件：E:\\itcast\\mn.jpg
                //获取数据源文件File对象的名称(mn.jpg)
                String srcFileName = srcFile.getName();
                //创建目的地文件File对象，路径名是目的地目录+mn.jpg组成(myCharStream\\itcast\\mn.jpg)
                File destFile = new File(destFolder,srcFileName);
                //复制文件
                copyFile(srcFile,destFile);
            }
        }

        private static void copyFile(File srcFile, File destFile) throws IOException {
            BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcFile));
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFile));

            byte[] bys = new byte[1024];
            int len;
            while ((len=bis.read(bys))!=-1) {
                bos.write(bys,0,len);
            }

            bos.close();
            bis.close();
        }


    }








package itheima_08;

import java.io.*;

    /*
    需求：
        把“E:\\itcast”复制到 F盘目录下

    思路：
        1:创建数据源File对象，路径是E:\\itcast
        2:创建目的地File对象，路径是F:\\
        3:写方法实现文件夹的复制，参数为数据源File对象和目的地File对象
        4:判断数据源File是否是目录
            是：
                A:在目的地下创建和数据源File名称一样的目录
                B:获取数据源File下所有文件或者目录的File数组
                C:遍历该File数组，得到每一个File对象
                D:把该File作为数据源File对象，递归调用复制文件夹的方法
            不是：说明是文件，直接复制，用字节流
 */
    public class CopyFoldersDemo {
        public static void main(String[] args) throws IOException {
            //创建数据源File对象，路径是E:\\itcast
            File srcFile = new File("E:\\itcast");
            //创建目的地File对象，路径是F:\\
            File destFile = new File("F:\\");

            //写方法实现文件夹的复制，参数为数据源File对象和目的地File对象
            copyFolder(srcFile,destFile);
        }

        //复制文件夹
        private static void copyFolder(File srcFile, File destFile) throws IOException {
            //判断数据源File是否是目录
            if(srcFile.isDirectory()) {
                //在目的地下创建和数据源File名称一样的目录
                String srcFileName = srcFile.getName();
                File newFolder = new File(destFile,srcFileName); //F:\\itcast
                if(!newFolder.exists()) {
                    newFolder.mkdir();
                }

                //获取数据源File下所有文件或者目录的File数组
                File[] fileArray = srcFile.listFiles();

                //遍历该File数组，得到每一个File对象
                for(File file : fileArray) {
                    //把该File作为数据源File对象，递归调用复制文件夹的方法
                    copyFolder(file,newFolder);
                }
            } else {
                //说明是文件，直接复制，用字节流
                File newFile = new File(destFile,srcFile.getName());
                copyFile(srcFile,newFile);
            }
        }

        //字节缓冲流复制文件
        private static void copyFile(File srcFile, File destFile) throws IOException {
            BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcFile));
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFile));

            byte[] bys = new byte[1024];
            int len;
            while ((len = bis.read(bys)) != -1) {
                bos.write(bys, 0, len);
            }

            bos.close();
            bis.close();
        }

    }






package itheima_09;

import javax.imageio.IIOException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/*
    复制文件加入异常处理
 */
public class CopyFileDemo {
    public static void main(String[] args) {

    }

    //JDK9的改进方案
    private static void method4() throws IOException {
        FileReader fr = new FileReader("fr.txt");
        FileWriter fw = new FileWriter("fw.txt");
        try(fr;fw){
            char[] chs = new char[1024];
            int len;
            while ((len = fr.read()) != -1) {
                fw.write(chs, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //JDK7的改进方案
    private static void method3() {
        try(FileReader fr = new FileReader("fr.txt");
            FileWriter fw = new FileWriter("fw.txt");){
            char[] chs = new char[1024];
            int len;
            while ((len = fr.read()) != -1) {
                fw.write(chs, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //try...catch...finally
    private static void method2() {
        FileReader fr = null;
        FileWriter fw = null;
        try {
            fr = new FileReader("fr.txt");
            fw = new FileWriter("fw.txt");

            char[] chs = new char[1024];
            int len;
            while ((len = fr.read()) != -1) {
                fw.write(chs, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fw!=null) {
                try {
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fr!=null) {
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    //抛出处理
    private static void method1() throws IOException {
        FileReader fr = new FileReader("fr.txt");
        FileWriter fw = new FileWriter("fw.txt");

        char[] chs = new char[1024];
        int len;
        while ((len = fr.read()) != -1) {
            fw.write(chs, 0, len);
        }

        fw.close();
        fr.close();
    }
}














package itheima_10;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Scanner;

    /*
        public static final InputStream in：标准输入流。通常该流对应于键盘输入或由主机环境或用户指定的另一个输入源
     */
    public class SystemInDemo {
        public static void main(String[] args) throws IOException {
            //public static final InputStream in：标准输入流
//        InputStream is = System.in;

//        int by;
//        while ((by=is.read())!=-1) {
//            System.out.print((char)by);
//}
            //如何把字节流转换为字符流？用转换流
//        InputStreamReader isr = new InputStreamReader(is);
//        //使用字符流能不能够实现一次读取一行数据呢？可以
//        //但是，一次读取一行数据的方法是字符缓冲输入流的特有方法
//        BufferedReader br = new BufferedReader(isr);

            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

            System.out.println("请输入一个字符串：");
            String line = br.readLine();
            System.out.println("你输入的字符串是：" + line);

            System.out.println("请输入一个整数：");
            int i = Integer.parseInt(br.readLine());
            System.out.println("你输入的整数是：" + i);

            //自己实现键盘录入数据太麻烦了，所以Java就提供了一个类供我们使用
            Scanner sc = new Scanner(System.in);
        }
    }








package itheima_10;

import java.io.PrintStream;

/*
    public static final PrintStream out: 标准输出流。通常该流对应于显示输出或由主机环境或用户指定的另一个输出目标
*/
public class SystemOutDemo {
    public static void main(String[] args) {
        //public static final PrintStream out: 标准输出流
        PrintStream ps = System.out;

        //能够方便地打印各种数据值
        //  ps.print("hello");
        //  ps.print(100);

        ps.println("hello");
        ps.println(100);

            // System.out的本质是一个字节输出流
        //System.out.println("hello");
        //System.out.println(100);

        System.out.println();
        //      System.out.print();

    }

}








package itheima_11;

import java.io.*;

public class CopyJavaDemo {
    /*
    需求：
        把模块目录下的PrintStreamDemo.java 复制到模块目录下的 Copy.java

    思路：
        1:根据数据源创建字符输入流对象
        2:根据目的地创建字符输出流对象
        3:读写数据，复制文件
        4:释放资源
 */

        public static void main(String[] args) throws IOException {
        /*
        //根据数据源创建字符输入流对象
        BufferedReader br = new BufferedReader(new FileReader("myOtherStream\\PrintStreamDemo.java"));
        //根据目的地创建字符输出流对象
        BufferedWriter bw = new BufferedWriter(new FileWriter("myOtherStream\\Copy.java"));

        //读写数据，复制文件
        String line;
        while ((line=br.readLine())!=null) {
            bw.write(line);
            bw.newLine();
            bw.flush();
        }

        //释放资源
        bw.close();
        br.close();
        */

            //根据数据源创建字符输入流对象
            BufferedReader br = new BufferedReader(new FileReader("Packday19-IO\\PrintStreamDemo.java"));
            //根据目的地创建字符输出流对象
            PrintWriter pw = new PrintWriter(new FileWriter("Packday19-IO\\Copy.java"),true);

            //读写数据，复制文件
            String line;
            while ((line=br.readLine())!=null) {
                pw.println(line);
            }

            //释放资源
            pw.close();
            br.close();
        }
    }










package itheima_11;

import java.io.IOException;
import java.io.PrintStream;

/*
    打印流的特点：
            只负责输出数据，不负责读取数据
            有自己的独特方法

    字节打印流
           PrintStream(String fileName):使用指定的文件名创建新的打印流
*/
public class PrintStreamDemo {
    public static void main(String[] args) throws IOException {
        //PrintStream(String fileName):使用指定的文件名创建新的打印流
        PrintStream ps = new PrintStream("packday19-IO\\ps.txt");

        //写数据
        //字节输出流有的方法
        //  ps.write(97);

        //使用特有方法写数据

        //  ps.print(97);
        //  ps.println();
        //  ps.print(98);

        ps.println(97);
        ps.println(98);



        //释放资源
        ps.close();
    }

}








package itheima_11;

import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

/*
    字符打印流的构造方法：
        PrintWriter(String fileName);使用指定的文件名创建一个新的PrintWriter,而不需要自动执行行刷新

        PrintWriter(Writer out,boolean autoFlush):创建一个新的PrintWriter
        out:字符输出流
        autoFlush:一个布尔值，如果为真，则println,printf,或format方法将刷新输出缓冲区
*/
public class PrintWriterDemo {
    public static void main(String[] args) throws IOException {
        //PrintWriter​(String fileName) ：使用指定的文件名创建一个新的PrintWriter，而不需要自动执行行刷新
//        PrintWriter pw = new PrintWriter("myOtherStream\\pw.txt");

//        pw.write("hello");
//        pw.write("\r\n");
//        pw.flush();
//        pw.write("world");
//        pw.write("\r\n");
//        pw.flush();

//        pw.println("hello");
        /*
            pw.write("hello");
            pw.write("\r\n");
         */
//        pw.flush();
//        pw.println("world");
//        pw.flush();

        //PrintWriter​(Writer out, boolean autoFlush)：创建一个新的PrintWriter
        PrintWriter pw = new PrintWriter(new FileWriter("myOtherStream\\pw.txt"),true);
//        PrintWriter pw = new PrintWriter(new FileWriter("myOtherStream\\pw.txt"),false);

        pw.println("hello");
        /*
            pw.write("hello");
            pw.write("\r\n");
            pw.flush();
         */
        pw.println("world");

        pw.close();
    }
}










package itheima_12;

import java.io.Serializable;
    public class Student implements Serializable {
        private static final long serialVersionUID = 42L;
        private String name;
        //    private int age;
        private transient int age;

        public Student() {
        }

        public Student(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

//    @Override
//    public String toString() {
//        return "Student{" +
//                "name='" + name + '\'' +
//                ", age=" + age +
//                '}';
//    }
    }













package itheima_12;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ObjectInputStreamDemo {
    /*
    构造方法：
        ObjectInputStream​(InputStream in)：创建从指定的InputStream读取的ObjectInputStream

    反序列化对象的方法：
        Object readObject​()：从ObjectInputStream读取一个对象
 */
        public static void main(String[] args) throws IOException, ClassNotFoundException {
            //ObjectInputStream​(InputStream in)：创建从指定的InputStream读取的ObjectInputStream
            ObjectInputStream ois = new ObjectInputStream(new FileInputStream("myOtherStream\\oos.txt"));

            //Object readObject​()：从ObjectInputStream读取一个对象
            Object obj = ois.readObject();

            Student s = (Student) obj;
            System.out.println(s.getName() + "," + s.getAge());

            ois.close();
        }
    }
    
    
    
    
    
    
    
    package itheima_12;

import IO.Student;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

/*
    对象序列化流
        构造方法：
            ObjectOutputStream​(OutputStream out)：创建一个写入指定的OutputStream的ObjectOutputStream

        序列化对象的方法：
            void writeObject​(Object obj)：将指定的对象写入ObjectOutputStream

    NotSerializableException:抛出一个实例需要一个Serializable接口。 序列化运行时或实例的类可能会抛出此异常

    类的序列化由实现java.io.Serializable接口的类启用。 不实现此接口的类将不会使任何状态序列化或反序列化
 */
public class ObjectOutputStreamDemo {
    public static void main(String[] args) throws IOException {
        //ObjectOutputStream​(OutputStream out)：创建一个写入指定的OutputStream的ObjectOutputStream
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("myOtherStream\\oos.txt"));

        //创建对象
        Student s = new Student("林青霞",30);

        //void writeObject​(Object obj)：将指定的对象写入ObjectOutputStream
        oos.writeObject(s);

        //释放资源
        oos.close();
    }
}








package itheima_12;

import java.io.*;

/*
        用对象序列化流序列化了一个对象后，假如我们修改了对象所属的类文件，读取数据会不会出问题呢？
            java.io.InvalidClassException:
                当序列化运行时检测到类中的以下问题之一时抛出。
                    类的串行版本与从流中读取的类描述符的类型不匹配
                    该类包含未知的数据类型
                    该类没有可访问的无参数构造函数

            com.itheima_03.Student; local class incompatible:
            stream classdesc serialVersionUID = -3743788623620386195,
            local class serialVersionUID = -247282590948908173


        如果出问题了，如何解决呢？
            给对象所属的类加一个值：private static final long serialVersionUID = 42L;

        如果一个对象中的某个成员变量的值不想被序列化，又该如何实现呢？
            private transient int age;
     */
    public class ObjectStreamDemo {
        public static void main(String[] args) throws IOException, ClassNotFoundException {
//        write();
            read();
        }

        //反序列化
        private static void read() throws IOException, ClassNotFoundException {
            ObjectInputStream ois = new ObjectInputStream(new FileInputStream("myOtherStream\\oos.txt"));
            Object obj = ois.readObject();
            Student s = (Student) obj;
            System.out.println(s.getName() + "," + s.getAge());
            ois.close();
        }

        //序列化
        private static void write() throws IOException {
            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("myOtherStream\\oos.txt"));
            Student s = new Student("林青霞", 30);
            oos.writeObject(s);
            oos.close();
        }
    }












package itheima_13;

import java.util.Random;
import java.util.Scanner;

    /*
    猜数字小游戏
 */
    public class GuessNumber {
        private GuessNumber() {
        }

        public static void start() {
            //要完成猜数字的游戏，首先需要有一个要猜的数字，使用随机数生成该数字，范围1到100
            Random r = new Random();
            int number = r.nextInt(100) + 1;

            while (true) {
                //使用程序实现猜数字，每次均要输入猜测的数字值，需要使用键盘录入实现
                Scanner sc = new Scanner(System.in);

                System.out.println("请输入你要猜的数字：");
                int guessNumber = sc.nextInt();

                //比较输入的数字和系统产生的数据，需要使用分支语句。这里使用if..else..if..格式，根据不同情况进行猜测结果显示
                if (guessNumber > number) {
                    System.out.println("你猜的数字" + guessNumber + "大了");
                } else if (guessNumber < number) {
                    System.out.println("你猜的数字" + guessNumber + "小了");
                } else {
                    System.out.println("恭喜你猜中了");
                    break;
                }
            }
        }
    }






package itheima_13;

import java.util.Properties;
import java.util.Set;

public class PropertiesDemo01 {
    /*
    Properties作为Map集合的使用
 */
        public static void main(String[] args) {
            //创建集合对象
//        Properties<String,String> prop = new Properties<String,String>(); //错误
            Properties prop = new Properties();

            //存储元素
            prop.put("itheima001", "林青霞");
            prop.put("itheima002", "张曼玉");
            prop.put("itheima003", "王祖贤");

            //遍历集合
            Set<Object> keySet = prop.keySet();
            for (Object key : keySet) {
                Object value = prop.get(key);
                System.out.println(key + "," + value);
            }
        }
    }







package itheima_13;

import java.util.Properties;
import java.util.Set;

public class PropertiesDemo02 {
    /*
    Properties作为集合的特有方法：
        Object setProperty(String key, String value)：设置集合的键和值，都是String类型，底层调用Hashtable方法put
        String getProperty(String key)：使用此属性列表中指定的键搜索属性
        Set<String> stringPropertyNames()：从该属性列表中返回一个不可修改的键集，其中键及其对应的值是字符串
 */
        public static void main(String[] args) {
            //创建集合对象
            Properties prop = new Properties();

            //Object setProperty(String key, String value)：设置集合的键和值，都是String类型，底层调用Hashtable方法put
            prop.setProperty("itheima001", "林青霞");
        /*
            Object setProperty(String key, String value) {
                return put(key, value);
            }

            Object put(Object key, Object value) {
                return map.put(key, value);
            }
         */
            prop.setProperty("itheima002", "张曼玉");
            prop.setProperty("itheima003", "王祖贤");

            //String getProperty(String key)：使用此属性列表中指定的键搜索属性
//        System.out.println(prop.getProperty("itheima001"));
//        System.out.println(prop.getProperty("itheima0011"));

//        System.out.println(prop);

            //Set<String> stringPropertyNames()：从该属性列表中返回一个不可修改的键集，其中键及其对应的值是字符串
            Set<String> names = prop.stringPropertyNames();
            for (String key : names) {
//            System.out.println(key);
                String value = prop.getProperty(key);
                System.out.println(key + "," + value);
            }
        }
    }









package itheima_13;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Properties;

/*
    Properties和IO流结合的方法：
        void load(Reader reader)：
            从输入字符流读取属性列表（键和元素对）

        void store(Writer writer, String comments)：
            将此属性列表（键和元素对）写入此 Properties表中，以适合使用 load(Reader)方法的格式写入输出字符流
 */
    public class PropertiesDemo03 {
        public static void main(String[] args) throws IOException {
            //把集合中的数据保存到文件
//        myStore();

            //把文件中的数据加载到集合
            myLoad();

        }

        private static void myLoad() throws IOException {
            Properties prop = new Properties();

            //void load(Reader reader)：
            FileReader fr = new FileReader("myOtherStream\\fw.txt");
            prop.load(fr);
            fr.close();

            System.out.println(prop);
        }

        private static void myStore() throws IOException {
            Properties prop = new Properties();

            prop.setProperty("itheima001","林青霞");
            prop.setProperty("itheima002","张曼玉");
            prop.setProperty("itheima003","王祖贤");

            //void store(Writer writer, String comments)：
            FileWriter fw = new FileWriter("myOtherStream\\fw.txt");
            prop.store(fw,null);
            fw.close();
        }
    }







package itheima_13;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Properties;

public class PropertiesTest {
    /*
    需求：请写程序实现猜数字小游戏只能试玩3次，如果还想玩，提示：游戏试玩已结束，想玩请充值(www.itcast.cn)

    思路：
        1:写一个游戏类，里面有一个猜数字的小游戏
        2:写一个测试类，测试类中有main方法，main()方法中按照下面步骤完成
            A:从文件中读取数据到Properties集合，用load()方法实现
                文件已经存在：game.txt
                里面有一个数据值：count=0
            B:通过Properties集合获取到玩游戏的次数
            C:判断次数是否到到3次了
                如果到了，给出提示：游戏试玩已结束，想玩请充值(www.itcast.cn)
                如果不到3次：
                    玩游戏
                    次数+1，重新写回文件，用Properties的store()方法实现
 */
        public static void main(String[] args) throws IOException {
            //从文件中读取数据到Properties集合，用load()方法实现
            Properties prop = new Properties();

            FileReader fr = new FileReader("myOtherStream\\game.txt");
            prop.load(fr);
            fr.close();

            //通过Properties集合获取到玩游戏的次数
            String count = prop.getProperty("count");
            int number = Integer.parseInt(count);

            //判断次数是否到到3次了
            if(number >= 3) {
                //如果到了，给出提示：游戏试玩已结束，想玩请充值(www.itcast.cn)
                System.out.println("游戏试玩已结束，想玩请充值(www.itcast.cn)");
            } else {
                //玩游戏
                GuessNumber.start();

                //次数+1，重新写回文件，用Properties的store()方法实现
                number++;
                prop.setProperty("count",String.valueOf(number));
                FileWriter fw = new FileWriter("myOtherStream\\game.txt");
                prop.store(fw,null);
                fw.close();
            }
        }
    }


