//calculator.h

#include<cstring>
#include<iostream>
#include<stack>
using namespace std;

class Calculator {
    private:
      stack<int> num;
      stack<char> op;
    public:
      int getResult(char str[]) {
          int size = strlen(str);
          int tmp = 0;
          int yxj[200] = {0};
          yxj['+'] = yxj['-'] = 1;
          yxj['*'] = yxj['/'] = 2;
          for (int i = 0; i < size; i++, tmp = 0) {
              if (str[i] <= '9' && str[i] >= '0') {
                  while (str[i] <= '9' && str[i] >= '0') {
                      tmp = tmp * 10 + str[i] - 48;
                      i++;
                  }
                  i--;
                  num.push(tmp);
              }
              if (str[i] == '(') op.push(str[i]);
              if (str[i] == '+' || str[i] == '-' || str[i] == '*'
                  || str[i] == '/') {
                   if (op.empty())  {
                       op.push(str[i]);
                   } else if (op.top() != '(' && yxj[str[i]] > yxj[op.top()]) {
                       op.push(str[i]);
                   } else if (op.top() != '(' && yxj[str[i]] <= yxj[op.top()]) {
                       int t;
                       while (!op.empty() && op.top() != '(' &&
                       yxj[str[i]] <= yxj[op.top()]) {
                           char top = op.top();
                           op.pop();
                           int a = num.top();
                           num.pop();
                           int b = num.top();
                           num.pop();
                           if (top == '+') t = a + b;
                           if (top == '-') t = b - a;
                           if (top == '*') t = a * b;
                           if (top == '/') t = b / a;
                           num.push(t);
                       }
                       op.push(str[i]);
                   } else if (op.top() == '(') {
                       op.push(str[i]);
                   }
              }
              if (str[i] == ')') {
                   while (op.top() != '(') {
                       char top = op.top();
                       op.pop();
                       int a = num.top();
                       num.pop();
                       int b = num.top();
                       num.pop();
                       if (top == '+') b += a;
                       if (top == '-') b -= a;
                       if (top == '*') b *= a;
                       if (top == '/') b /= a;
                       num.push(b);
                   }
                   op.pop();
              }
          }
          while (!op.empty()) {
              char top = op.top();
              op.pop();
              int a = num.top();
              num.pop();
              int b = num.top();
              num.pop();
              if (top == '+') b += a;
              if (top == '-') b -= a;
              if (top == '*') b *= a;
              if (top == '/') b /= a;
              num.push(b);
          }
          return num.top();
      }
};

//main.cpp

#include <iostream>
#include "calculator.h"
using std::endl;
 
int main() {
Calculator c;
char str[100];
cin >> str;
cout << c.getResult(str) << endl;
}
