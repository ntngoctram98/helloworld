///#TRUE TABLE
#include <iostream>
#include <list>
#include <stack>
#include <string>
#include <math.h>
#include <fstream>
#include <stdlib.h>
using namespace std;

string columnTitle(list <char>, list <char>)
{
  
  return 0;
}

string OneLineConseq(list <char>, int* bits, list <char>)
{
  return 0;
}


bool readFile(char* nameFile, list <char> & ex)
{
  bool res;
  ifstream in; char x;
  in.open(nameFile, ios::in);

  if (in.is_open())
  {

    while (!in.eof())
    {
      in >> x;
      /*COPY tu dong 40 bo do ***** if(s[i] == '~')*/
      ex.push_back(x);//
      res = true;
    }
    in.close();
  }
  else
    res = false;
  return res;
}

//Vi be Cham hong co visual nen gia su doc file nhu vay nhe ^-^
bool readFileDemo(string s, list <char> & prefix) 
{
  int i = 0;
  while (s[i] != '\0')
  {
    if(s[i] == '~')
    {
      prefix.push_back('2');
      i++;
    }
    else if(s[i] == '^')
    {
      prefix.push_back('3');
      i++;
    }
    else if(s[i] == 'v')
    {
      prefix.push_back('4');
      i++;
    }
    else if(s[i] == '-')
    {
      prefix.push_back('5');
      i += 2;
    }
    else if(s[i] == '<')
    {
      prefix.push_back('6');
      i += 3;
    }
    else
    {
      prefix.push_back(s[i]);
      i++;
    }
  }
  return true;
}
       
char priority(char x)
{
  char res;
  if(x == '(')
    res = '0';
  else if(x == ')')
    res = '1';
  else if(x == '~')
    res = '2';
  else if(x == '^')
    res = '3';
  else if(x == 'v')
    res = '4';
  else if(x == '-')//->
    res = '5';
  else if(x == '<')//<->
    res = '6';
  
  return res;
}
//varList: danh sach bien
//LUU TEN BIEN
int cntVar(list <char> & ex, list <char> & varList)
{
  int size = ex.size();
  int cnt = 0; char x;
 
  for (int i = 0; i < size; i++)
  {
    x = *ex.begin();
    if (x >= 97 && x <= 122 && x != 'v')
    {
      cnt++;
      varList.push_back(x);     
    }
    ex.pop_front();
  }
  return cnt;
}

int reversePolish(list <char> & prefix, list <char> & postfix)
{
  int sz = prefix.size();
  stack <char> unVar;
  for (int i = 0; i < sz; i++)
  {
    char x = *prefix.begin();
    if(x >= 97 && x <= 122 && x != 'v')
      postfix.push_back(x);
    else if(x == '(')
      unVar.push(x);
    else if(x == ')')
    {
      char tmp;
      while(!unVar.empty())
      {
        tmp = unVar.top();
        if(tmp == '(')
        {
          unVar.pop();
          break;
        }
        else
        {
          postfix.push_back(tmp);
          unVar.pop();
        }
      }
    }
    else if(x == '2' || x == '3' || x == '4' ||x == '5'||x == '6')
    {
      int c = int(x) - 48;
      if(unVar.empty())
        unVar.push(x);
      else      
      {
        char endElementSt = unVar.top();
        char pri = priority(endElementSt);
        int e = int(pri) - 48;
          if (c > e)
            unVar.push(x);
          else
          {
            postfix.push_back(endElementSt);
            unVar.pop();
          }
      } 
      
    } 
    prefix.pop_front();
  }
  while(!unVar.empty())
  {
    char tmp0 = unVar.top();
    postfix.push_back(tmp0);
    unVar.pop();
  }
  return 0;
}
void dec2Bin(int n, int a[], int col)
{
  int *tmp = new int[col];
  int i;
  for(i = 0; i < col; i++)
  {
    tmp[i] = n % 2;
    n/=2;
  }
  //in ra khong bi nguoc
  int j = 0;
  for(i = i - 1; i >= 0; i--)
  {
    a[j] = tmp[i];
    j++;
  }
}


int **binaryLine(int n)
{
  int r = pow(2, n);
  //cout << "r: " << r << endl;
  int **a;
  a = new int*[r];
  for(int i = 0; i < r; i++)
    a[i] = new int[n]; 
  
  int *tmp = new int[n];
  
  for (int i = 0; i < r; i++)
  {
    dec2Bin(i, tmp, n);
    for (int j = 0; j < n; j++)
      a[i][j] = tmp[j];
  }  
  //in mang
  for (int i = 0; i < r; i++)
  {
    for (int j = 0; j < n; j++)
      cout << a[i][j] << " ";
    cout << "\n";
  }  
  return a;
}

int main()
{
  list <char> pos, pre, var, preCopy;

  //****************** READ_FILE ******************
  //char nameFile[] = "logicexpression.txt";
  //readFile(nameFile, pre);
  string s = "(~p->r)^(q->r)<->(p->q)->r";//"((tvq)^~p)<->(z->(uvq))";
  readFileDemo(s, pre);
  list <char> :: iterator it;
  //copy them 1 list pre
  for(it = pre.begin(); it != pre.end(); ++it)
  {
    preCopy.push_back(*it);
    cout << *it << ' ';
  }
  
  //****************** COUNT_VARIABLE ******************
  int cnt = cntVar(preCopy, var);
  cout << "\nThere are " << cnt <<" variable."<< endl;
  for(it = var.begin(); it != var.end(); ++it)
    cout << *it << ' ';
  cout << '\n' << endl;
  
  //****************** BINARY_LINE ******************
  binaryLine(2);
  
  //****************** REVERSE_POLISH ******************
  reversePolish(pre, pos);
  for(it = pos.begin(); it != pos.end(); ++it)
    cout << *it << ' ';
  cout << '\n';  
  return 0;
}
