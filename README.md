# -#include<iostream>
using namespace std;

/*定义车，车站*/
typedef struct
{
	int stopnum;
	int carnum;
	double gettime;
	double outtime;
}Car,stopcar;

/*定义入站,出站队列*/
typedef struct
{
	int front;
	int rear;
	int num;
	Car c1[3];
}isqueue, osqueue;

/*出站*/
void outstop(osqueue  *cx, int time)
{
	cx->front++;
	cx->c1[cx->front].outtime = time;
	cout << "汽车号码为" << cx->c1[cx->front].carnum
		<< "汽车停放的位置" << cx->c1[cx->front].stopnum
		<< "汽车开始停放的时间" << cx->c1[cx->front].gettime
		<< "汽车离开的时间" << cx->c1[cx->front].outtime
		<< "共产生费用" << ((cx->c1[cx->front].outtime - cx->c1[cx->front].gettime) /60)*3 << "元"<<endl;
	cx->c1[cx->front].carnum = 0;
	cx->c1[cx->front].gettime = 0;
	cx->c1[cx->front].outtime = 0;
	cx->c1[cx->front].stopnum = 0;
	cx->front++;
	
}

/*入站*/
void instop(isqueue *& cx,Car c2,int i)
{
	int a[7] = { 111,112,113,114,115,116,117 };
	cx->rear++;
	cx->c1[cx->rear].carnum = c2.carnum;
	cx->c1[cx->rear].gettime = c2.gettime;
	cx->c1[cx->rear].stopnum = a[i];
	cout << "汽车号码为" << cx->c1[cx->rear].carnum
		<< "汽车停放的位置" << cx->c1[cx->rear].stopnum
		<< "汽车开始停放的时间" << cx->c1[cx->rear].gettime << endl;
	cx->rear++;

}


/*查询空车位*/
int finenull(stopcar* car)
{
	int i = -1;
	for (int j = 0; j < 7; j++)
	{
		if (car[j].carnum == 0 && car[j].gettime == 0 && car[j].outtime == 0 && car[j].stopnum == 0)
		{
			i = j;
			return i;
		}

	}
	return -2;
}

/*寻找空车位*/
void findnull(stopcar *car)
{
	int i = -1;
	int n = 0;
	int a[7] = { 111,112,113,114,115,116,117 };
	for (int j = 0; j < 7; j++)
	{
		
		if (car[j].carnum == 0 && car[j].gettime == 0 && car[j].outtime == 0 && car[j].stopnum == 0)
		{
			i = j;
			cout <<a[i]  << "\t";
			n++;
		}
	}
	if (n > 0)
	{
		return;
	}
	else
	{
		cout << "没有车位了" << endl;
	}

	
}
/*初始化队列*/
void InitQueue(isqueue*& q)
{
	
	q = (isqueue*)malloc(sizeof(isqueue));
	if (q == NULL)
	{
		return;
	}
	q->front = -1;
	q->rear = -1;
	for (int j = 0; j < 3; j++)
	{
		q->c1[j].carnum = 0;
		q->c1[j].gettime = 0;
		q->c1[j].outtime = 0;
		q->c1[j].stopnum = 0;
	}
}

/*外界车进车队*/
bool enQueue(isqueue*& q, Car e)
{
	q->rear++;
	if (q->rear >= 3 )
	{
		cout << "入队列已满" << endl;
		return false;
	}
	
	q->c1[q->rear].carnum = e.carnum;
	q->c1[q->rear].gettime = e.gettime;
	return true;
}

/*入队车队进车库*/
void deQueue(isqueue*& q,stopcar *cx, double time,int i)
{
	int a[7] = { 111,112,113,114,115,116,117 };
	q->front++;
	cx[i].carnum = q->c1[q->front].carnum;
	cx[i].gettime = time;
	cx[i].stopnum = a[i];
	
}

/*入车队的移动*/
void incarmove(isqueue*& q)
{
	q->front = 0;
	//cout << q->front;
	
		
		q->c1[q->front ].carnum = q->c1[(q->front)+1].carnum;
		q->c1[q->front ].gettime = q->c1[(q->front) +1].gettime;
		q->c1[q->front ].outtime = q->c1[(q->front) +1].outtime;
		q->c1[q->front ].stopnum = q->c1[(q->front) +1].stopnum;
		q->c1[(q->front)+1].carnum = q->c1[(q->front) + 2].carnum;
		q->c1[(q->front) + 1].carnum = q->c1[(q->front) + 2].carnum;
		q->c1[(q->front) + 1].gettime = q->c1[(q->front) + 2].gettime;
		q->c1[(q->front) + 1].outtime = q->c1[(q->front) + 2].outtime;
		q->c1[(q->front) + 1].stopnum = q->c1[(q->front) + 2].stopnum;
	q->front = -1;
	q->rear -= 1;
	q->c1[2].carnum = 0;
	q->c1[2].gettime = 0;
	q->c1[2].outtime = 0;
	q->c1[2].stopnum = 0;
}
/*出车队的移动*/
void outcarmove(isqueue*& q)
{
	//cout << q->front;
	while (q->c1[q->front].carnum != 0&&q->c1[q->front].gettime!=0)
	{
		//q->front++;
		q->c1[q->front].carnum = q->c1[q->front + 1].carnum;
		q->c1[q->front].gettime = q->c1[q->front + 1].gettime;
		q->c1[q->front].outtime = q->c1[q->front + 1].outtime;
		q->c1[q->front].stopnum = q->c1[q->front + 1].stopnum;
		q->front++;
	}
	q->front = -1;
	q->rear -= 1;
}


/*车库到出队列*/
bool boutstop(osqueue*& q, stopcar* cx, int i)
{
	q->rear++;
	if (q->rear >= 3 )
	{
		cout << "出队列已满" << endl;
		return false;
	}
	q->c1[q->rear].carnum = cx[i].carnum ;
	q->c1[q->rear].gettime = cx[i].gettime;
	q->c1[q->rear].outtime = cx[i].outtime;
	q->c1[q->rear].stopnum = cx[i].stopnum;
	cx[i].carnum = 0;
	cx[i].gettime = 0;
	cx[i].outtime = 0;
	cx[i].stopnum = 0;
	return true;
}
/*打印停车场*/
void dispstop(stopcar* ca)
{
	cout << "当前停车场状态" << endl;
	for (int j = 0; j < 7; j++)
	{
		cout << " 车辆号码" << ca[j].carnum << "车辆停放位置" << ca[j].stopnum <<"车辆开始停放时间"<<ca[j].gettime<< endl;
	}
}

/*打印入站队列*/
void dispsqueue(isqueue* queue)
{
	cout << "当前等待入库车辆" << endl;
	for (int j = 0; j < 3; j++)
	{
		cout << " 车辆号码" << queue->c1[j].carnum << "车辆停放位置" << queue->c1[j].stopnum << "车辆开始停放时间" << queue->c1[j].gettime << endl;
	}
}

/*打印出站队列*/
void dispsqueueout(isqueue* queue)
{
	cout << "当前等待计费车辆" << endl;
	for (int j = 0; j < 3; j++)
	{
		cout << " 车辆号码" << queue->c1[j].carnum << "车辆停放位置" << queue->c1[j].stopnum << "车辆开始停放时间" << queue->c1[j].gettime << endl;
	}
}
/*判断车站是否为满*/
bool findnum(stopcar* cx)
{
	for (int j = 0; j < 7; j++)
	{
		if (cx[j].carnum == 0&&cx[j].gettime==0&&cx[j].stopnum==0)
		{
			return true;
		}
	}
	return false;
}

/*判断车队是否第一位有车*/
bool findonull(osqueue* q2)
{
	if (q2->c1[0].carnum != 0)
	{
		return true;
	}	
	else
	{
		return false;
	}
}

/*转换函数（入队）*/
int tran(int& a, int& b)
{
	int c[7] = { 111,112,113,114,115,116,117 };
	for (int j = 0; j < 7; j++)
	{
		if (c[j] == a)
		{
			b = j;
		}
	}
	return b;
}
int main()
{
	stopcar car[7] = { {0,0,0},{112,2,1220},{113,3,1220},{114,4,1220} ,{115,5,1220} ,{116,6,1220} ,{117,1,1220} };
	Car c1 ;
	isqueue *q1;
	osqueue *q2;
	InitQueue(q1);
	InitQueue(q2);
	int n = 0;
	cout << "1.入站" << " 2.出站"  <<" 3.计费" <<" 4.打印出队列"" 5.查询停车场车辆"<< " 6.打印入队列"<< "7.退出系统"<<endl;
	while (cin >> n)
	{
	cout << "1.入站" << " 2.出站" << " 3.计费" << " 4.打印出队列"" 5.查询停车场车辆" << " 6.打印入队列" << "7.退出系统" << endl;
		switch (n)
		{
		/*入站*/
		case 1:
		{
			cout << "请输入车牌号" << endl;
			int num;
			cin >> num;
			c1.carnum = num;
			if (findnum(car))//判断车库满没满，如果没满则车辆直接进入
			{
				cout << "请输入入站时间" << endl;
				int time1;
				cin >> time1;
				c1.gettime = time1;
				enQueue(q1, c1);
				cout << "当前拥有车位" << endl;
				findnull(car);
				cout << endl;
				cout << "你要在哪停车" << endl;
				int b;
				cin >> b;
				int c;
				tran(b, c);
				deQueue(q1, car, q1->c1[0].gettime, c);
				incarmove(q1);
				dispsqueue(q1);
				cout << endl;
				dispstop(car);
				c1.gettime = 0;
				break;
			}
			if (enQueue(q1, c1))//车库满了，车辆排队
				dispsqueue(q1);


			break;
		}
		/*出站*/
		case 2:
		{
			int i;
			int stopnumx;

			cout << "请输入出场车辆车牌号" << endl;
			cin >> i;
			/*寻找车辆停放的车位*/
			for (int j = 0; j < 7; j++)
			{
				if (car[j].carnum == i)
				{
					stopnumx = j;
				}
			}
			if (boutstop(q2, car, stopnumx))
			{
				int b = finenull(car);
				if (findonull(q1))
				{
					cout << "有汽车入站" << endl;
					cout << "请输入入站时间" << endl;
					int time2;
					cin >> time2;
					q1->c1[0].gettime = time2;
					deQueue(q1, car, q1->c1[0].gettime, b);
					incarmove(q1);
				}

				dispsqueueout(q2);
				break;
			}
			break;
		}
		/*计费*/
		case 3:
		{
			if (!findonull(q2))//判断出队列是否为空
			{
				cout << "当前无车出队" << endl;
				break;
			}
			cout << "请输入出站时间" << endl;
			int time2;
			cin >> time2;
			outstop(q2, time2);
			incarmove(q2);//车队移动
			break;
		}
		/*打印出队列*/
		case 4:
			dispsqueueout(q2);
			break;
		/*打印停车场*/
		case 5:
			dispstop(car);
			break;
		/*打印入队列*/
		case 6:
			dispsqueue(q1);
			break;
		case 7:
			goto out;
			break;
		default:
			cout << "输入错误请重新输入" << endl;
			break;
		}
	}
	cout << "输入有错误，系统崩溃" << endl;
out:
	cout << "系统已经退出" << endl;
	return 0;
}
