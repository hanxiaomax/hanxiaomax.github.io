T& operator[ ](int i){return i<0||i>last?NULL:slist[i];} 

 查找算法
 1.折半
 2.递归折半
 3.散列hash查找

 排序算法
 1.插入排序
 2.交换排序
 3.选择排序
 4.合并排序

 多个构造函数？
 静态成员：调用，初始化，均不同于一般的。
```
 /*==========================================================================================
    Reference code of Matrix class
    The 1st one in week 8
    Sample by Xiaoguo Zhang
    School of INS, SEU

【题目】
        参考PPT辛普森积分法的实现，设计矩形法计算积分的基类类CIntegration，
    派生之，使支持sin(x)以及1/(1+x*x+sin(x)两个函数的积分，编写测试程序计算
    两个函数在[0，20]区间的积分值。

【解题关键】
    （1）本题要求用矩形法计算积分，因此不要抄袭Sinperson算法的代码。
         矩形法 S = sigma(f(from + i * delta_x)*delta_x)
     (2) 在基类中实现积分的统一算法，由于fun()函数本身是虚拟的，那么即使在基类
         函数中调用这个fun()函数，也可以自动触发派生类的版本（多态）
         这样保证了，只要在派生类中派生一下fun（）,就可以自动计算积分


==========================================================================================*/

#include <iostream>
#include <cmath>
using namespace std;

class CIntegration
{
public:
    virtual double fun(double x) //
    {
        return 0;
    }
    CIntegration(double from1,   //  积分下限  
                 double to1,     //  积分上限   
                 int slice_num1 = 20000) // 切片个数
                :from(from1),to(to1),slice_num(slice_num1)  //初始化序列
    {
    }
    
    double Integration(); // 计算积分的函数，返回积分值，需要你实现，矩形法的基本原理见上图
protected:
    double from;        // 积分下限
    double to;          // 积分上限
    int    slice_num;   // 积分切片个数
};
//完成支持计算sin(x)的类CSinIntegration, 支持1/(1+x*x+sin(x)积分的类CFractionIntegration，并用如下测试函数测试：
double CIntegration::Integration()
{
    double S = 0, dx = 0;
    int i = 0;
    
    dx=(to-from)/slice_num;
    
    S = 0;
    for (i = 0 ; i < slice_num ; i ++)
    {
        S += fun(from + dx * i ) * dx;
    }
    return S;
}

class CSinIntegration :public CIntegration
{
public:
    CSinIntegration (double from1=0 ,
                     double to1=0):
                     CIntegration(from1,to1)
    {
             
    }
    double fun(double x)
    {
        return sin(x);
    }
};

class CFractionIntegration :public CIntegration
{
public:
    CFractionIntegration (double from1=0 ,
                          double to1=0)
                          :CIntegration(from1,to1)
    {
    }
    double fun(double x)
    {
        return 1/(1+x*x+sin(x));
    }
};

int main()
{
    CSinIntegration a1(0,20);
    CFractionIntegration b1(0,20);

    CIntegration *s1 = &a1;
    CIntegration *s2 = &b1;

    cout<<"integration of sin(x) = "<<s1->Integration()<<endl;
    cout<<"integration of 1/(1+x*x+sin(x)= "<<s2->Integration()<<endl;

    return 0;
}
```

```
#include <iostream>
using namespace std;

class CCadElement
{
public:
    CCadElement(){cout<<"CCadElement()"<<endl;}
    virtual ~CCadElement(){cout<<"~CCadElement()"<<endl;}
public:
    virtual void Move(double dx, double dy) {};
    virtual void Draw() {};
    void Mirror() { cout<<"CCadElement::Mirror() called"<<endl; }
}; 

class CCircle : public CCadElement
{
public:
    CCircle(int ox1, int oy1, double r1):ox(ox1),oy(oy1),r(r1)
    {
        cout<<"CCircle()"<<endl;
    }
    virtual ~CCircle(){cout<<"~CCircle()"<<endl;}
    void Move(double dx, double dy) 
    {
        ox+=dx;
        oy+=dy;
    }
    void Draw ()
    {
        cout<<"圆心为("<<ox<<","<<oy<<") "<<"半径为"<<r<<endl;
    }
    void Mirror()
    {
        cout<<"CCircle::Mirror() called"<<endl;
        ox=-ox;
    }
protected:
    double ox;
    double oy;
    double r;
};

class CRectangle : public CCadElement
{
public:
    CRectangle(int ox1, int oy1, double l1, double d1):ox(ox1),oy(oy1),l(l1),d(d1)
    {
        cout<<"CRectangle()"<<endl;
    }
    virtual ~CRectangle(){cout<<"~CRectangle()"<<endl;}
    void Move(double dx, double dy) 
    {
        ox+=dx;
        oy+=dy;
    }
    void Draw ()
    {
        cout<<"左下坐标为("<<ox<<","<<oy<<") "<<"长为"<<l<<" 宽为"<<d<<endl;
    }
    void Mirror()
    {
        cout<<"CRectangle::Mirror() called"<<endl;
        ox=-ox;
    }
protected:
    double ox;
    double oy;
    double l;
    double d; 
};

//（2）测试函数参考：
int main()
{
    CCircle c1(1,0,5); //圆心为（1，0），半径5
    CRectangle  r1(1,2,3,4); //左下（1，2），宽3，高4
    //测试一
    c1.Move(1,2);  // 此时圆心为(2,2)  
    c1.Draw();    // 把圆心及半径信息输出到屏幕即可
    c1.Mirror();  // 此时圆心为(-2,2)
    c1.Draw();    // 把圆心及半径信息输出到屏幕即可

    //测试二
    r1.Move(1,2);  //此时左下(2,4)
    r1.Draw();    // 把圆心及半径信息输出到屏幕即可
    r1.Mirror();    //此时左下变成（-2,4）
    r1.Draw();     //把矩形信息输出到屏幕即可

    // 下面用指针测试的代码省略，自己写
    CCircle c2(1,0,5); //圆心为（1，0），半径5
    CRectangle  r2(1,2,3,4); //左下（1，2），宽3，高4

    CCadElement* pElement = &c2;
    //测试三
    pElement->Move(1,2); 
    pElement->Draw();    // 把圆心及半径信息输出到屏幕即可
    pElement->Mirror();
    pElement->Draw();    // 

    //测试四
    pElement = &r2;
    pElement->Move(1,2);  
    pElement->Draw();
    pElement->Mirror();  
    pElement->Draw();     
}
```

