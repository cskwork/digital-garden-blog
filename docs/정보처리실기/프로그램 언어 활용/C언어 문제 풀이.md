========================================
```c
#include <stdio.h>
void main() {
    int c=0;
    int i=0;
	
	while(i<10) {
		i++;
		c *= i;
	}
    printf("%d", c);
}

// DECLARE, RUN, RESULT 
// 1-10 

// RESULT
// 0
```
========================================

```c
#include 
void main(){
  char *p = "KOREA"
  
  printf("%sn" , p);
  printf("%sn" , p+3);
  printf("%cn" , *p);
  printf("%cn" , *(p+3));
  printf("%cn" , *p+2);
}

// KOREA 
// EA
// K
// E
// M
```
========================================

```c
#include 
// struct jsu {...} defines a structure named "jsu" that contains four elements
struct jsu {
  char name[12];
  int os, db, hab, hhab;
};
 
int main(){
  // Inside main(), an array of three jsu structures is created and initialized
	//st[0]: nae="데이터1", os=95, db=88
	//st[1]: nae="데이터2", os=84, db=91
	//st[2]: nae="데이터3", os=86, db=75
  
  
  struct jsu st[3] = { 
				  {"데이터1", 95, 88}, 
				  {"데이터2", 84, 91}, 
				  {"데이터3", 86, 75}
					 };
  // pointer to jsu structure (struct jsu* p) is declared
  struct jsu* p;
 
  p = &st[0];
  
  // st[1].hab = st[1].os + st[2].db
  // st[1].hab = 84 + 75 = 159
  (p + 1)->hab = (p + 1)->os + (p + 2)->db;
  
  // st[1].hhab = st[1].hab + st[0].os + st[0].db 
  // st[1].hhab = 159 + 95 + 88 = 342
  (p + 1)->hhab = (p+1)->hab + p->os + p->db;
 
  printf("%dn", (p+1)->hab + (p+1)->hhab); 
  
  // RESULT 
  // 501
}
```
========================================

```c
int mp(int base, int exp);
int main(){
   int res;
   res = mp(2,10);
   printf("%d",res);
   return 0;
}

int mp(int base, int exp) {
   int res = 1;
   for(int i=0; i < exp; i++){
      res = res * base;
   }
   
   return res;
}
```
