#Visual C++ Programming (B)

##2017 –  2018 学年第 1 学期 电信系

###第二周编程练习

----------------
----------------           
###@author:电信1506 李卓 '01201509351008
----------------
----------------


### 0.myMark标志打印效果图
该功能由myMark()函数完成，其打印的效果如下图所示：

![maMark.PNG](http://upload-images.jianshu.io/upload_images/7292832-10e3e1990d0133c5.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 1.结构体与函数  ###
#### 1.1 总体思路
步骤2中要求实现一个以自定义的结构体类型为返回值的函数，其功能是对结构体中数组完成初始化。我们可以以结构体作为该函数的参数传进去，并在函数的末尾返回该结构体；虽然传进去的和返回的是同一个结构体，但是在函数内部已经对它进行了“加工”，此过程中完成初始化。

#### 1.2 运行结果
结果如下图所示：

![结构体与函数.PNG](http://upload-images.jianshu.io/upload_images/7292832-ba77a0ba7c9144a3.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.3 代码结构
代码如下，其中注释已经比较详细，不在赘述：

    struct matrix {
    	int row = 4;
    	int col = 4;
    	int array[4][4];
    }mat1;//定义结构体，并用此结构体定义一个变量；
    
    void myMark(void) {
    	//警告！此部分内容切勿修改或删除，否则程序将产生不可预知的错误！！
    	/***************输出自己的标志**************/
    	cout << endl;
    	cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
    	cout << "本程序由 李卓-0121509351008 完成" << endl
    	 	 << "武汉理工大学信息学院 电信1506 班" << endl
    	 	 << "VC++程序设计编程练习" << endl<<endl;
    	cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
    }
    
    matrix ini_matrix(matrix m) {
    	int count = 12;//矩阵初始化时的起始值，此处的12并非硬性要求值，可更改；
    	for (int i = 0; i < 4; i++) {//初始化矩阵；
    		for (int j = 0; j < 4; j++) {
    			m.array[i][j] = count++;//count每使用一次，自增1；
    			//cout << m.array[i][j] << "  ";//初始化同时输出初始化的值，相关功能调整到函数"testStruct"中去；
    			//if (count % 4 == 0) {//对应于count初值的12，每4个换行一次，输出为矩阵形式；
    				//cout << endl;
    			//}
    		}
    		//cout << endl;//这个上面又费事了，SB了我...
    	}
    	return m;//返回结构体类型，这里虽然传进来一个m，又返回去一个m，但是返回去的m已经经过了加工；
    }
    
    void testStruct(void) {
    	cout << "矩阵的行数为：" << ini_matrix(mat1).row << "矩阵的列数为：" << ini_matrix(mat1).col << endl
    		<< "矩阵的内容为：" << endl;
    	int temp = 0;//临时变量，方便打印矩阵时换行
    	for (int i = 0; i < 4; i++) {
    		for (int j = 0; j < 4; j++) {
    			cout << ini_matrix(mat1).array[i][j] << "  ";//打印矩阵内容
    			temp++;
    			if (temp % 4 == 0) {
    				cout << endl;//每打印4个值，还一行打印
    			}
    			//cout << endl;//其实上面的换行完全不必那么麻烦，只需在此处打印换行符即可，同时可免去定义临时变量temp的麻烦
    		}
    	}
    }


其中testStruct()中的

	cout << ini_matrix(mat1).array[i][j] << "  ";

这一句即是传进去一个未初始化的结构体，经过“加工”（即初始化）之后将它本身返回，并通过".array[][]"的操作实现结构体中数组内容的访问。

---------------------------
---------------------------
### 2.指针 ###
#### 2.1 总体思路
C++中，将数组名解释为地址，并指向数组中第一个元素，所以数组名实际上就是一个指向数组首地址的指针。定义一个指向数组某元素的地址的指针可以使用这样的语句：

	int *p = &a[i];
其中i的范围时从0到数组的长度减1。但是值得注意的是，在联系中我发现，对于char型数组，当使用cout进行输出操作时，若使用 p + i;
来得到地址，其结果总时错误的，中有当使用printf()函数进输出时才时正确的，这与其他的类型的数组都不相同。若使用

	int *p = &a;
则此时指针指向的时整个数组的首地址，若此时再使用

	cout << p + 1;
则此时输出的将不再是数组第二个元素的地址，而将是在原数组的首地址的基础上加上整个数组的长度的一个地址。
若要取某指针指向的数组的某元素的地址，则应当使用这样的语句：

	cout << p + i;
若要使用该指针指向的数组的元素的内容，需要使用的语句是：

	 *(p + i) = a;
	cout << *(p + i);

#### 2.2 运行结果
运行结果的截图如下图所示：

![指针.PNG](http://upload-images.jianshu.io/upload_images/7292832-dab9bd139b22b7c5.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.3 代码结构

    void testPointer(void) {
    	//以下4行分别定义了char,int,double,matrix类型的数组，且其长度都为4；
    	char myChar[4];
    	int myInt[4];
    	double myDouble[4];
    	matrix myMatrix[4];
    
    	//以下4行分别定义4个指针，分别指向上述的四种类型的数组的首元素；
    	char *c = &myChar[0];
    	int *i = &myInt[0];
    	double *d = &myDouble[0];
    	struct matrix *mp = &myMatrix[1];
    
    	cout << "char类型占用的字节数为:"<< sizeof (*c) << endl;
    	cout << "int类型占用的字节数为:" << sizeof (*i) << endl;
    	cout << "double类型占用的字节数为:" << sizeof (*d) << endl;
    	cout << "matrix类型占用的字节数为:" << sizeof (*mp) << endl;
    
    	cout << "-------------"<< endl;
    
    	printf("数组myChar的各元素地址为:%d   %d  %d  %d \n", c, c + 1, c + 2, c + 3);//测试发现char型数组和其他的竟然不一样！
    	//+若使用不带取地址符号(&)的指针变量，将输出乱码，这里为保证所使用的符号的一致，使用了printf()；函数进行输出；
    	cout << "数组myInt的各元素的地址为:" << i << "" << i + 1 << "" << i + 2 << "" << i + 3 << endl;
    	cout << "数组myDouble的各元素的地址为:" << d << "" << d + 1 << "" << d + 2 << "" << d + 3 << endl;
    	cout << "数组mtMatrix的各元素的地址为：" << mp << "    " << mp + 1 << "     " << mp + 2 << "     " << mp + 3 << endl;
    
    	cout << endl << "-------------" << endl;
    
    	cout << "char型数组赋值：" << endl;
    	for (int i = 0; i < 4; i++) {
    		//这里直接借助临时的循环变量本身的自增性完成数组的初始化；
    		*(c + i) = 'a' + i;//'a'+1应该是'b','a'+2应该是'c'等等；
    		cout << *(c + i) << "   ";
    	}
    	cout << endl << "----------------" << endl;
    
    	cout << "int型数组赋值：" << endl;
    	for (int j = 0; j < 4; j++) {
    		//这里直接借助临时的循环变量本身的自增性完成数组的初始化；
    		*(i + j) = 1 + j;
    		cout << *(i + j) << "";
    	}
    	cout << endl << "----------------" << endl;
    
    	cout << "double型数组赋值：" << endl;
    	srand(unsigned(time(0)));//埋下一颗种子??(*^_^*)??；
    	for (int k = 0; k < 4; k++) {
    		//这里直接借助临时的循环变量本身的自增性完成数组的初始化；
    		//每循环一次，种子开花、结果一次；
    		*(d + k) = rand() % 100 / 10.0 ;
    		//利用种子函数生成随机数，借助"100/10.0"这个运算巧妙地得到了小数；
    		cout << *(d + k) << "   ";
    	}
    	cout << endl << "-------------------" << endl;
    
    	cout << "为结构体matrix中的数组的赋值：" << endl;
       
    	for (int i = 0; i < 4; i++) {
    		ini_matrix(*(mp + i));
    		//调用上面定义的结构体初始化函数完成对结构体数组中的4个结构体的初始化，通过指针传递参数（即个结构体本身）；
    		cout << "结构体maMatrix[" << i << "]初始化完成..." << endl;
    		//方便查看这里仅形式化输出，实际上也完成了初始化，但初始化的同时并没有输出而已；
    	}
    	cout << endl << "------------------" << endl << endl;
    }

---------------------------
---------------------------
### 3. 汉诺塔
#### 3.1总体思路
汉诺塔时一个典型的递归问题，递归的问题最终都是化为最简单的、已解决的“一”的问题。例如在此中的问题中，当只有一个盘子时，通过一步的移动即可解决：即起点-->终点。当有两个盘子时，两个盘子是“二”的问题，将“二”的问题转化为三个“一”的问题，而“一”的问题我们已经解决。当有n个盘子时，先将最上面的n-1个盘子看作整体，这样就转化为了“二”的问题，而“二”的问题我们在上面已经解决；再将上述的n-1单独对待，将其转化为n-2的问题，这样一直递归下去，直到转化为最先的“二”的问题，即被解决。
值得注意的是，函数的参数列表的顺序、位置的重要性在这里被充分的展现了出来了：这里的递归性调用中，虽然都是同一个函数，但是由于每次进行的参数的不同，实际节能型的参数传递时不同的，这也就实现了将“二”的问题华为三个“一”的问题，最终解决了问题。

#### 3.2运行结果
运行结果的截图如下图所示：

![汉诺塔.PNG](http://upload-images.jianshu.io/upload_images/7292832-bcd8441a23985b47.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 3.3代码结构

    void hanoi(int n, char outset, char buff, char destination){
    	//参数的位置和顺序很重要，实际进行参数传递的时候，在参数在哪个位置上，就是哪个的功能，不能"望名知义"；
    	if (n == 1) {
    		cout << outset << "(移出柱子)" << "---> " << destination << "(移入柱子)" << endl;
    		//递归的问题最终都是化为最简单的、已解决的“一”的问题；
    		//当只有一个盘子时，通过一步的移动即可解决：即起点-->终点；
    		//当有两个盘子时，两个盘子是“二”的问题，将“二”的问题转化为三个“一”的问题，“一”的问题我们已经解决：通过下述的	三步操作可解决问题；
    		//当有n个盘子时，先将最上面的n-1个盘子看作整体，这样就转化为了“二”的问题，而“二”的问题我们在上面已经解决；
    		//再将上述的n-1单独对待，将其转化为n-2的问题，这样一直递归下去，直到转化为“一”的问题；
    	}
    	else{
    		//转化为n-1的问题；
    		hanoi(n - 1, outset, destination, buff);//这里交换了destination和buff的位置顺序，则实际上时从outset移动到buff；
    		hanoi(n - 1, outset, buff, destination);
    		hanoi(n - 1, buff, outset, destination);//这里交换了outset和buff的位置顺序，则实际上时从buff移动到destination；
    	}
    }

----------------------
----------------------
### 4. 项目整体的代码组织结构
该项目的整体代码结构如下图所示：

![总体结构图.PNG](http://upload-images.jianshu.io/upload_images/7292832-c8a1aba158cc0846.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中所有的功能函数由

	showMeThis();

函数调用。

主函数部分分别调用了

	myMark();
	showMeThis();
这两个函数，完成了整体的功能。
关于整体的组织结构的组织形式，附代码如下：


	#include <iostream>
    #include<stdlib.h>
    #include<ctime>//随机种子所需；
    
    using namespace std; 
    
    void showMeThis(void) {
    	/*
    	*一、结构体与函数
    	*/
    	cout << " /**********一、结构体与函数*********/ " << endl;
    	testStruct();
    	/*
    	*二、指针
    	*/
    	cout << "/*************二、指针************************/" << endl;
    	testPointer();
    	/*
    	*三、汉诺塔
    	*/
    	cout << "/**************三、汉诺塔*********************/" << endl;
    	int dis_num = 4;
    	cout << "此时共有4个盘子！" << endl;
    	hanoi(dis_num, 'A', 'B', 'C');//进行参数传递时,实际上最初的出发点是'A'，终点是'C'，buff是'B'；
    } 
    
    void  main(void) {
    	/*
    	*打印我的标记
    	*/
    	myMark();
    	/*
    	*功能函数区
    	*/
    	showMeThis();
    	/*
    	*等待从键盘键入，暂留控制台以供查看；
    	*/
    	cin.get();
    }
    

---------------------------    
首次编辑于：2017/9/17 20:42:42     
最后编辑于：2017/9/19 9:17:34 