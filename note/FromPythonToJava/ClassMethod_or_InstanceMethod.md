From CSDN


如同类的成员变量有实例变量和类变量两种一样，类的方法也有两种：实例方法和类方法。在方法定义时，冠以修饰字static的方法称为类方法，没有冠以static修饰字的方法是实例方法。

1.类D定义了一个实例方法，两个类方法。

class D{
    int a;//实例变量
    static int c;//类变量
    float max(int x,int y)//实例方法
        ｛a = x>y?x:y;｝
    static int setCount(int c0)//类方法
        { c=c0;}
    static void incCount(int step)//类方法
        ｛c+=step;｝
}
类中的实例方法可以互相调用，并可调用类方法。类方法也能相互调用，但不能直接调用实例方法，除非类方法引入局部对象，然后通过局部对象调用实例方法。另外，类方法能直接引用类变量，不能引用实例变量。实例方法可引用实例变量，也可引用类变量。

2.给出的类声明中有些是合法的代码，而有些是不合法的代码。

class E{
    float u;
    static float v;
    static void setUV(boolean f){
        u = s_m(f);// 非法，类方法不可以调用实例变量u
        v = s_m(f);//合法，类方法可以调用类方法
        v = r_m(!f);//非法，类方法不能直接调用实例方法。
    }
    static float s_m(boolean f){
        return f?u:v;//非法，类方法只能 类变量
    }
    float r_m(boolean f){
        return f?u:v;//合法，实例方法能引用实例变量和类变量
    }
}
实例方法可以访问类变量和当前对象的实例变量。实例方法必须通过对象调用，不能通过类名调用。类方法只能类变量，
不能够访问实例变量。类方法除了可以通过实例对象调用之外，还可以通过类名调用。

3.说明类变量用法的，应用程序。改写Point类的声明，在Point类中增加一个类变变量pCount,它的初值为0。在构造方法中，有类变量pCount增1的代码，这能记录类的对象个数。

public class Example3_2{
public static void main(String args[]){
    Point p1,p2,p3;
    p1 = new Point();
    p2 = new Point(40,50);
    p3 = new Point(p1.getX()+p2.getX(),p1.getY()+p2.getY());
    System.out.println(“p3.x=”+p3.getX()+”,p3.y=”+p3.getY());
    Point p4 = new Point(p1.x,p2.y);
    System.out.println(“p4.x=”+p4.x+”,p4.y=”+p4.y);
    System.out.println(“程序共有Point对象”+Point.pointNum()+ “个”);
    }
}
class Point{
    int x,y;
    static int pCount =0;
Point(){
    x=10;
    y=20;
    pCount++;
}
Point(int x,int y){
    this.x = x;
    this.y=y;
    pCount++;
}
static int pointNum(){return pCount;}
    int getX(){ return x; }
    int getY(){ return y; }
}
由于java系统内设废弃内存回收程序，所以一般情况下，一个对象使用结束后，程序不必特别通知系统撤销对象。但有时为提高系统资源的利用率，程序也可通过调用方法finialize()显式通知系统，请求系统撤销对象。
————————————————

                            版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
                        
原文链接：https://blog.csdn.net/qq_36900609/article/details/87630987