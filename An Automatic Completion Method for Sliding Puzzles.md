#include <iostream>
using namespace std;
int user_num[25]={100,1,2,3,5,4,6,11,21,16,7,8,10,9,12,22,17,13,15,14,23,18,19,20,24}; //내가 정한 순위
int user_num_2[25]={100,1,2,3,5,4,6,10,11,13,12,7,14,17,19,18,9,16,21,22,23,8,15,20,24}; //내가 정한 순위에 따른 우선순위

class puzzle{       //슬라이드 퍼즐 클래스    
		int num[25]={17,1,5,7,16,12,21,6,20,4,10,24,2,9,23,3,15,13,22,14,18,8,11,19,0};
		public:		
		puzzle();        //생성자    
		void printpuz();  //콘솔창에 퍼즐 출력    
		void complete_message(); //결과 메세지    
		void move(int a);  //a번 자리의 퍼즐 이동    
		void change(int a, int b); // 두 자리의 퍼즐을 교환
    int find(int a);  // 25개 배열을 탐색해 a의 자리를 반환  
		
		void makeway(int x, int a, int b);  //0을 이동시켜 길을 만든다. (a에서 b로 만든다.)    
		void locate(int a, int b); //a를 제자리에 위치시킨다. a를 b자리로 옮긴다.    
		void locate_2(int a); //a를 제자리에 위치시킨다. 행, 열의 끝부분 처리.    
		void complete_puz(); // 순번대로 locate나 locate_2함수를 동작시킨다.
};

puzzle::puzzle(){ //생성자   
		printpuz();
}

void puzzle::printpuz(){  //콘솔창에 퍼즐 출력    
		int a;   
		cout<<"---------------"<<endl;  
		for(a=0; a<25; a++){    
				if(num[a]==0) cout<<"   ";    
				else if(num[a]<10) cout<<num[a]<<"  ";   
				else     cout<<num[a]<<" ";    
				if(a%5==4) cout<<endl;        //5개씩 끊어서 출력    
		}
}

void puzzle::complete_message(){    
		if(num[18]!=19||num[19]!=20||num[23]!=24){       
				cout<<"---------------"<<endl;    
				cout<<"샘로이드 법칙에 의해"<<endl;     
				cout<<"존재할 수 없는 퍼즐 배열입니다."<<endl;  
				cout<<"23과 24의 자리를 바꾼 후"<<endl;     
				cout<<"다시 실행하세요"<<endl;     
				cout<<"---------------"<<endl;
    }
    cout<<"---------------"<<endl;    
		cout<<"  * 완   성 *"<<endl; 
		cout<<"---------------"<<endl;
}

void puzzle::change(int a, int b){    
		int temp; 
		temp = num[a];
		num[a] = num[b];   
		num[b] = temp;
}

void puzzle::move(int a){   // a번 자리의 퍼즐 이동   
		if(a%5!=0&&num[a-1]==0){  //왼쪽이 0이면 왼쪽과 교환      
				change(a-1, a);  
		}   
		else if (a%5!=4&&num[a+1]==0){ //오른쪽이 0이면 오른쪽과 교환        
				change(a+1, a);   
		}   
		else if (a>4 && num[a-5]==0){  //위쪽이 0이면 위쪽과 교환   
				change(a-5, a);   
		}  
		else if (a<20&&num[a+5]==0) {//아래쪽이 0이면 아래쪽과 교환    
				change(a+5, a);  
		}    
		else{       
				cout<<"오류"<<endl;       
				for(int i=0; i<10000000000;i++){} 
		}  
		printpuz();
}

int puzzle::find(int a){   
		int b; 
		for(b=0; b<25; b++){      
				if(a==num[b]){       
						break;      
				}      
				else continue;   
		}  
		return b;
}

void puzzle::makeway(int x, int a, int b){ //x타겟 a에서 b로 0을 이동시킴    
		int up = a/5 - b/5;    
		int right = b%5 - a%5;    
		if((up>1||up<-1) && right==0)  //상하로 일자면 옆으로 하나 옮겨줘야함   
		{    
				if(find(0)%5==4){       
						move(find(0)-1);      
						right++;  
				}   
				else{ 
						move(find(0)+1);  
						right--;  
				}   
				if(up>0){ 
						move(find(0)-5);
						up--;
				} 
				else if(up<0){   
						move(find(0)+5); 
						up++;
				} 
		} 
		else if(up==0 && (right>1||right<-1))  //좌우로 일자인 경우 상하로 하나 옮겨줘야함    
		{ 
				if(find(0)>19){ 
						move(find(0)-5); 
						up--; 
				} 
				else{ 
						move(find(0)+5);  
						up++;
				} 
				if(right>0){ 
						move(find(0)+1);  
						right--;   
				} 
				else if(right<0){
						move(find(0)-1);  
						right++;
				}
		} 
		while(1){     
				if(up==0 && right==0){  
						break;   
				}
				if(up>0 && user_num_2[num[find(0)-5]]>user_num_2[x]){      
						move(find(0)-5);
						up--;  
						continue;   
				}
				else if(right>0 && user_num_2[num[find(0)+1]]>user_num_2[x]){ 
						move(find(0)+1); 
						right--;    
						continue;   
				} 
				else if(up<0 && user_num_2[num[find(0)+5]]>user_num_2[x]){    
						move(find(0)+5);  
						up++;    
						continue; 
				}
				else if(right<0 && user_num_2[num[find(0)-1]]>user_num_2[x]){ 
						move(find(0)-1);
						right++;   
						continue;  
				} 
				else{  //예외처리    
						if(find(0)-1==find(x)){ 
								move(find(x)+6);
								move(find(x)+5); 
								move(find(x)+4);
								move(find(x)-1);   
								right+=2; 
						} 
						else if(find(0)+1==find(x)){
								move(find(x)+4);    
								move(find(x)+5);    
								move(find(x)+6);            
								move(find(x)+1);            
								right-=2;               
						} 
						else if(find(0)-5==find(x)){   
								move(find(x)+6);          
								move(find(x)+1);           
								move(find(x)-4);          
								move(find(x)-5);           
								up-=2;                
						} 
						else if(find(0)+5==find(x)){          
								move(find(x)-4);                  
								move(find(x)+1);                  
								move(find(x)+6);                  
								move(find(x)+5);            
								up+=2;           
						}
				}
		}
}

void puzzle::locate(int a, int b){  //a를 b자리로. 보통경우 a=b  
		int up = find(a)/5 - (b-1)/5;  
		int right = (b-1)%5 - find(a)%5;  
		while(1){        
				if(up==0 && right==0){       
						break;
				}
				if(up>0 && user_num_2[num[find(a)-5]]> user_num_2[a]){ //위로 가야하고,  윗 숫자가 타겟숫자보다 순번이 낮을 때     
						makeway(a, find(0), find(a)-5);     
						move(find(a));     
						up--;
						continue;    
				}
				else if(right>0 && user_num_2[num[find(a)+1]]> user_num_2[a]){//오른쪽으로 가야하고, 오른쪽 숫자가 타겟숫자보다 순번이 낮을 때   
						makeway(a, find(0), find(a)+1);      
						move(find(a));        
						right--;        
						continue;        
				} 
				else if(up<0 && user_num_2[num[find(a)+5]]> user_num_2[a]){//아래로 가야하고, 아랫숫자가 타겟숫자보다 순번이 낮을 때       
						makeway(a, find(0), find(a)+5);    
						move(find(a));      
						up++;        
						continue; 
				}   
				else if(right<0 && user_num_2[num[find(a)-1]]> user_num_2[a]){//왼쪽으로 가야하고, 왼쪽숫자가 타겟숫자보다 순번이 낮을 때   
						makeway(a, find(0), find(a)-1);      
						move(find(a));   
						right++;       
						continue;     
				}   
		}
}

void puzzle::locate_2(int a){  // 4,5같은 마지막경우처리 
		if(a%5==4){    
				makeway(a, find(0), a);       
				move(find(0)-1);  
				move(find(0)+5);  
		}   
		if(a>15&&a<=20){
        makeway(a, find(0), a+4);   
				move(find(0)-5);    
				move(find(0)+1); 
		}
}

void puzzle::complete_puz(){  
		int a; 
		for(a=1; a<=21; a++){    
				if(user_num[a]%5==0){        
						locate(user_num[a],user_num[a]-1); //ex 5면 4자리  
				}       
				else if(user_num[a]%5==4){      
						if(find(user_num[a])==user_num[a]){          
								makeway(user_num[a], find(0), find(user_num[a])+5);          
								move(find(0)-5);         
								move(find(0)-1);         
								move(find(0)+5);         
								move(find(0)+1);         
								move(find(0)+5);         
								move(find(0)-1);         
								move(find(0)-5);         
								move(find(0)-5);         
								move(find(0)+1);         
						}         
						locate(user_num[a],user_num[a]+5); // ex 4면 4밑에자리로       
						locate_2(user_num[a]);   
				}      
				else if(user_num[a]>20&&user_num[a]<=25){ //ex21이면 16자리로    
						locate(user_num[a],user_num[a]-5);      
				}      
				else if(user_num[a]>15&&user_num[a]<=20){//요거 //ex 16이면 16옆자리
            if(find(user_num[a])==user_num[a]+4){      
								makeway(user_num[a], find(0), find(user_num[a])+1);          
								move(find(0)-1);       
								move(find(0)-5);       
								move(find(0)+1);       
								move(find(0)+5);       
								move(find(0)+1);       
								move(find(0)-5);       
								move(find(0)-1);       
								move(find(0)-1);       
								move(find(0)+5);       
						}        
						locate(user_num[a],user_num[a]+1);        
						locate_2(user_num[a]);     
				}       
				else locate(user_num[a],user_num[a]);   
		}  
		locate(19,19); 
		makeway(1,find(0),24); 
		complete_message();
}

int main(){  
		puzzle a1; 
		a1.complete_puz();  
		return 0;
}
