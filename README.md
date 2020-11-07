# STUSYS.V3.0
//This is one of my assignments for practice courses.
#include <stdio.h>
#include <stdlib.h>
#include<string.h>
#define N 30
#define Class_Num 6
//输入系统
void InputSystem(long lStudentID[N],float fTestScore[N][Class_Num],char cStudentName[N][N],float fScoreInput[N][Class_Num],long lIDInput[N],char NameInput[N][N],int *p,int *mark);
//用来按行输出考试成绩
void Output_row(float a[][Class_Num],int m);
//用来按行交换的函数
void Row_Swap(float fTestScore[][Class_Num],int m);
//用来交换两个字符串的函数
void cSwap(char *a,char *b);
//用来交换数字的函数
void Swap(float *a,float *b);
//用来求总分的函数
void Summarize(float fTestScore[][Class_Num],int iStudentNumber,float fSum_of_Class[],float fSum_of_Student[]);
//用来按照测试成绩排出表
void Sort_by_Test_Score(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],float (*compare)(float a,float b),int iStudentNumber,float fSum_of_Student[]);
//用来按照学生成绩排序
void Sort_by_Student_ID(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],float (*compare)(float a,float b),int iStudentNumber,float fSum_of_Student[]);
//用来按照学生姓名排序
void Sort_by_Student_Name(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],int iStudentNumber,float fSum_of_Student[]);
//升序和降序
float Ascending(float a,float b);
float Descending(float a,float b);
//按照分数段分类函数
void Classify(float fTestScore[][Class_Num],int iStudentNumber,
              int *Sup,int *High,int *Mid,int *Pass,int *Fail);
//按照学生学号查询函数
void Select_by_Student_ID(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],int iStudentNumber,long ID,float fSum_of_Student[]);
//按照学生姓名查询函数
void Select_by_Student_Name(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],int iStudentNumber,char NAME[],float fSum_of_Student[]);
int Num_Class=0;
int main()
{
    long lStudentID[N]=    {0};                          //学生学号
    float fTestScore[N][Class_Num]=   {0};               //学生成绩
    char cStudentName[N][N];                             //学生姓名
    float fScoreInput[N][Class_Num]=  {0};              //输入成绩时的顺序
    long lIDInput[N]=      {0};                          //输入学号时的顺序
    char NameInput[N][N];                               //输入时姓名的顺序

    float fSum_of_Class[Class_Num]={0};                 //每门课程的总分
    float fSum_of_Student[N]={0};                       //每个学生的总分

    int iSupremeRank=0;                                 //优秀
    int iHighRank=0;                                    //良好
    int iMidRank=0;                                     //中等
    int iPassRank=0;                                    //及格
    int iFailRank=0;                                    //不及格
    int iStudentNumber=0;                               //学生人数
    int *piStudentNumber;     piStudentNumber=&iStudentNumber;

    int order=100,mark=0;                               //指令和mark
    int *pMark;     pMark=&mark;                        //用来标记是否录入学生信息的mark及其指针

    do
    {
        order=0;
        printf("Please input your order:\n");
        printf("1. Input Record\n2. List record\n3.Calculate total and average score of the course\n");
        printf("4.Sort in descending order by score\n5.Sort in ascending order by score\n");
        printf("6.Sort by Ascending order by Student ID\n7.Sort in dictionary order by name\n8.Search by Student ID\n9.Search by name\n10.Statistic analysis\n0.Exit\n");
        scanf ("%d",&order);
        fflush(stdin);

        //输入order为1则开始录入学生成绩
        if(order==1)
        {
            InputSystem(lStudentID,fTestScore,cStudentName,fScoreInput,lIDInput,NameInput,piStudentNumber,pMark);
        }
        //保证优先输入学生数据
        if(order!=0&&mark==0&&order!=1)
        {
            printf("Please input the records first!\n");
            continue;
        }
        //已完成学生成绩输入且order为2则按原顺序输出
        else if(mark==1&&order==2)
        {
            for(int m=0;m<iStudentNumber;m++)
            {
                printf("StudentID: %ld\t Score: %.2f\n",lIDInput[m],fScoreInput[m]);
                Output_row(fTestScore,m);
            }
            continue;
        }
        //计算并显示课程的总分和平均分
        else if(mark==1&&order==3)
        {
            Summarize(fTestScore,iStudentNumber,fSum_of_Class,fSum_of_Student);
            fflush(stdin);
            continue;
        }
        //按成绩由高到低排出名次表
        else if(mark==1&&order==4)
        {
            Sort_by_Test_Score(cStudentName,lStudentID,fTestScore,Descending,iStudentNumber,fSum_of_Student);
            for(int i=0; i<iStudentNumber; i++)
            {
                printf("Student name:%s \t Student id :%ld \tScore : %.2f\n",cStudentName[i],lStudentID[i],fSum_of_Student[i]);
                Output_row(fScoreInput,i);
            }

            fflush(stdin);
            continue;
        }
        //按成绩由低到高输出名次表
        else if(mark==1&&order==5)
        {
            Sort_by_Test_Score(cStudentName,lStudentID,fTestScore,Ascending,iStudentNumber,fSum_of_Student);
            for(int i=0; i<iStudentNumber+1; i++)
            {
                if(lStudentID[i]==0||fTestScore[i]==0)continue;
                printf("Student name:%s \t Student id :%ld \tScore : %.2f\n",cStudentName[i],lStudentID[i],fSum_of_Student[i]);
                Output_row(fTestScore,i);
            }
            fflush(stdin);
            continue;
        }
        //按学号由小到大排出成绩表
        else if(mark==1&&order==6)
        {
            Sort_by_Student_ID(cStudentName,lStudentID,fTestScore,Ascending,iStudentNumber,fSum_of_Student);
            for(int i=0; i<iStudentNumber+1; i++)
            {
                if(lStudentID[i]==0||fTestScore[i]==0)continue;
                printf("Student name:%s \t Student id :%ld \tScore : %.2f\n",cStudentName[i],lStudentID[i],fSum_of_Student[i]);
                Output_row(fTestScore,i);
            }
            fflush(stdin);
            continue;
        }
        //按姓名的字典顺序排出成绩表
        else if(mark==1&&order==7)
        {
            Sort_by_Student_Name(cStudentName,lStudentID,fTestScore,iStudentNumber,fSum_of_Student);
            for(int i=0; i<iStudentNumber+1; i++)
            {
                if(lStudentID[i]==0||fTestScore[i]==0)continue;
                printf("Student Name: %s\t Student id:%ld\t Score:\t%.2f \n",cStudentName[i],lStudentID[i],fSum_of_Student[i]);
                Output_row(fTestScore,i);
            }
            fflush(stdin);
            continue;
        }
        //按学号查找查询学生排名及成绩
        else if(mark==1&&order==8)
        {
            long ID=0;
            printf("Please input a student ID");
            scanf("%ld",&ID);
            Select_by_Student_ID(cStudentName,lStudentID,fTestScore,iStudentNumber,ID,fSum_of_Student);
            continue;
            fflush(stdin);
        }
        //按学生姓名查找
        else if(mark==1&&order==9)
        {
            char NAME[N]="";
            printf("Please input the student's name:\n");
            fgets(NAME,N,stdin);
            Select_by_Student_Name(cStudentName,lStudentID,fTestScore,iStudentNumber,NAME,fSum_of_Student);
        }
        //分类
        else if(mark==1&&order==10)
        {
            Classify(fTestScore,iStudentNumber,&iSupremeRank,&iHighRank,&iMidRank,&iPassRank,&iFailRank);
            fflush(stdin);
        }
        if (!(order>=0&&order<=10))printf("Please input a right order!!!!!\n");
        fflush(stdin);
    }
    while(order!=0);


    return 0;
}
//用来按行输出学生成绩的函数
void Output_row(float a[][Class_Num],int m)
{
    for(int i=0;i<Num_Class;i++)
    {
        printf("第%d门课程分数为%f\n",i+1,a[m][i]);
    }
}
//输入系统
void InputSystem(long lStudentID[N],float fTestScore[N][Class_Num],char cStudentName[N][N],float fScoreInput[N][Class_Num],long lIDInput[N],char NameInput[N][N],int *p,int *mark)
{
    printf("Please input the number of the classes:\n");
    scanf("%d",&Num_Class);                         //录入课程数目
    for(int i=0; i<30; i++)
    {

        printf("Please input the student id:\n");
        scanf("%ld",&lStudentID[i]);                    //录入学生学号
        lIDInput[i]=lStudentID[i];                      //用另一个数组记录学生学号
        fflush(stdin);
        printf("Please input the Student's Name:\n");
        fgets(cStudentName[i],N,stdin);//输入学生姓名
        strlwr(&cStudentName[i]);                       //把学生姓名转换为小写
        strcpy(&NameInput[i],&cStudentName[i]);         //用另一个数组保存输入顺序
        fflush(stdin);

        int j=0;
        do
        {
            printf("Please input the score of course %d:\n",j+1);
            scanf("%f",&fTestScore[i][j]);
            fScoreInput[i][j]=fTestScore[i][j];         //将输入的成绩保存并拷贝
            j=j+1;
            fflush(stdin);
        }while(fTestScore[i][j]>0||j<Num_Class);

        if(lStudentID[i]<0)
        {
            lIDInput[i]=lStudentID[i]=fTestScore[i][j]=fScoreInput[i][j]=0;//将非法输入如负数等变为0
            *p=i;
            *mark=1;
            fflush(stdin);
            break;                                      //输入负数表示输入结束
        }
    }
}

//字符串交换
void cSwap(char *a,char *b)
{
    char temp[N];
    strcpy(temp,a);
    strcpy(a,b);
    strcpy(b,temp);
}
//乾坤大挪移函数 shi ne
void Swap(float *a,float *b)
{
    float temp=*a;
    *a=*b;
    *b=temp;
}
//实现按行交换数组元素
void Row_Swap(float fTestScore[][Class_Num],int m)
{
    for(int i=0;i<Class_Num-1;i++)
    {
        Swap(&fTestScore[m][i],&fTestScore[m+1][i]);
    }
}

//用于求总分和平均分的函数
void Summarize(float fTestScore[][Class_Num],int iStudentNumber,float fSum_of_Class[],float fSum_of_Student[])
{
    for(int i=0;i<Num_Class;i++)
    {
        fSum_of_Class[i]=0;
        for(int j=0;j<iStudentNumber;j++)
        {
            fSum_of_Class[i]=fTestScore[j][i]+fSum_of_Class[i];
        }
        printf("课程%d总分为%f,平均分为%f\n",i+1,fSum_of_Class[i],fSum_of_Class[i]/iStudentNumber);
    }                                                           //求每门课的总分

    for(int i=0;i<iStudentNumber;i++)
    {
        fSum_of_Student[i]=0;
        for(int j=0;j<Num_Class;j++)
        {
            fSum_of_Student[i]=fTestScore[i][j]+fSum_of_Student[i];
        }
        printf("学生%d总分为%f,平均分为%f\n",i+1,fSum_of_Student[i],fSum_of_Student[i]/Num_Class);
    }                                                           //计算每门课的总分
}
//用于按照分数来升序或降序排序的函数
void Sort_by_Test_Score(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],float (*compare)(float a,float b),int iStudentNumber,float fSum_of_Student[])
{
    for(int i=0; i<iStudentNumber; i++)
    {
        for(int j=0; j<iStudentNumber-1; j++)
        {
            if((*compare)(fSum_of_Student[j],fSum_of_Student[j+1]))
            {
                cSwap(&cStudentName[j],&cStudentName[j+1]);//交换学生姓名
                Row_Swap(fTestScore,j);//交换学生成绩
                Swap(&lStudentID[j],&lStudentID[j+1]);//交换学生ID
                Swap(&fSum_of_Student[j],&fSum_of_Student[j+1]);
            }
            else continue;
        }
    }
}
//用来实现按学号排序的函数
void Sort_by_Student_ID(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],float (*compare)(float a,float b),int iStudentNumber,float fSum_of_Student[])
{
    for(int i=0; i<iStudentNumber; i++)
    {
        for(int j=0; j<=iStudentNumber-1; j++)
        {
            if((*compare)(lStudentID[j],lStudentID[j+1]))
            {
                cSwap(&cStudentName[j],&cStudentName[j+1]);//交换学生姓名
                Swap(&lStudentID[j],&lStudentID[j+1]);//交换学生ID
                Row_Swap(fTestScore,j);//交换学生成绩
                Swap(&fSum_of_Student[j],&fSum_of_Student[j+1]);
            }
            else continue;
        }
    }
}
//按字典顺序排序函数
void Sort_by_Student_Name(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],int iStudentNumber,float fSum_of_Student[])
{
    for(int i=0; i<iStudentNumber; i++)
    {
        for(int j=0; j<=iStudentNumber-1; j++)
        {
            if(strcmp(cStudentName[j],cStudentName[j+1])>0)
            {
                cSwap(&cStudentName[j],&cStudentName[j+1]);
                Swap(&lStudentID[j],&lStudentID[j+1]);
                Row_Swap(fTestScore,j);
                Swap(&fSum_of_Student[j],&fSum_of_Student[j+1]);
            }
            else continue;
        }
    }
}
//这样输入是升序排序
float Ascending(float a,float b)
{
    return a>b;
}
//这样输入是降序排序
float Descending(float a,float b)
{
    return a<b;
}

//分类函数
void Classify(float fTestScore[][Class_Num],int iStudentNumber,
              int *Sup,int *High,int *Mid,int *Pass,int *Fail)
{
    for(int j=0; j<Num_Class; j++)                  //遍历每门课的遍历每个学生
    {
        *Sup=0;*High=0;*Mid=0;*Pass=0;*Fail=0;
        for(int i=0; i<iStudentNumber; i++)
        {
            if(fTestScore[j][i]>=90&&fTestScore[j][i]<=100)
            {
                *Sup=*Sup+1;                        //90到100的为针的狠不戳
            }
            if(fTestScore[j][i]>=80&&fTestScore[j][i]<90)
            {
                *High=*High+1;                      //80到九十为针不戳
            }
            if(fTestScore[j][i]>=70&&fTestScore[j][i]<80)
            {
                *Mid=*Mid+1;                        //七十到八十为不戳
            }
            if(fTestScore[j][i]>=60&&fTestScore[j][i]<70)
            {
                *Pass=*Pass+1;                      //六十到七十为还行
            }
            if(fTestScore[j][i]>=0&&fTestScore[j][i]<59)
            {
                *Fail=*Fail+1;                      //不及格就是ko no dio da!
            }
        }
        printf("第%2d门科目统计情况\nSupremeRank=%d,\nHighRank=%d,\nMidRank=%d,\nPassRank=%d,\nFailRank=%d\n",j+1,*Sup,*High,*Mid,*Pass,*Fail);
    }

}
//按学号查找函数
void Select_by_Student_ID(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],int iStudentNumber,long ID,float fSum_of_Student[])
{
    int found=0;
    Sort_by_Test_Score(cStudentName,lStudentID,fTestScore,Descending,iStudentNumber,fSum_of_Student);//先排序一遍，以便输出排名
    for(int i=0; i<iStudentNumber; i++)
    {
        if(lStudentID[i]==ID)//当学号和id相等时，输出其信息
        {
            printf("Student name: %s\t ID: %ld\t Ranking: %d\t TotalScore: %.2f\n",cStudentName[i],ID,i+1,fSum_of_Student[i]);//输出学生排名（即i+1）和成绩（当然是由高到低的排名）
            Output_row(fTestScore,i);//按行输出成绩
            found=1;
        }
        if(found!=1&&i==iStudentNumber-1){printf("NOT FOUND!\n");}//如果没找到，则输出找不到这个野仔啦！
    }
}
//按学生姓名查找
void Select_by_Student_Name(char cStudentName[][N],long lStudentID[],float fTestScore[][Class_Num],int iStudentNumber,char NAME[],float fSum_of_Student[])
{
    int found=0;
    Sort_by_Test_Score(cStudentName,lStudentID,fTestScore,Descending,iStudentNumber,fSum_of_Student);
    strlwr(NAME);                           //将输入的NAME转换为小写，便于查询
    for(int i=0; i<iStudentNumber; i++)
    {
        if(strcmp(NAME,cStudentName[i])==0)//当输入的NAME和学生匹配时，输出其信息
        {
            printf("Student name: %s\t ID: %ld\t Ranking: %d\t TotalScore: %.2f\n",cStudentName[i],lStudentID[i],i+1,fSum_of_Student[i]);//输出学生排名（即i+1）和成绩（当然是由高到低的排名）
            Output_row(fTestScore,i);
            found=1;
        }
        if(found!=1&&i==iStudentNumber-1){printf("NOT FOUND!\n");}//如果没找到，则输出找不到这个野仔啦!
    }
}

