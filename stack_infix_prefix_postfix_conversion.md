# **Expression Conversion Notes**

## **1. Expression Types**
### **1.1 Infix Expression:**
- Operators are written between operands.
- Example: `A + B`
- **Easy to read but requires parentheses to specify precedence.**

### **1.2 Prefix Expression (Polish Notation):**
- Operators appear before operands.
- Example: `+ A B`
- **Eliminates the need for parentheses.**

### **1.3 Postfix Expression (Reverse Polish Notation - RPN):**
- Operators appear after operands.
- Example: `A B +`
- **Easier for computation (used in stack-based evaluation).**

---

## **2. Conversion Techniques**
### **2.1 Infix to Postfix Conversion**
#### **Algorithm**:
1. Use a stack to store operators and maintain precedence.
2. If an operand appears, add it directly to the output.
3. If an operator appears:
   - Pop from the stack to the output **until** the stack is empty or an operator with lower precedence is found.
   - Push the current operator onto the stack.
4. If `(` appears, push it to the stack.
5. If `)` appears, pop from the stack to output **until** `(` is encountered.
6. At the end, pop all remaining operators from the stack to the output.

#### **Code (C++)**:
```cpp
#include <bits/stdc++.h>
using namespace std;

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

string infixToPostfix(string s) {
    stack<char> st;
    string result;
    for (char c : s) {
        if (isalnum(c))
            result += c;
        else if (c == '(')
            st.push(c);
        else if (c == ')') {
            while (!st.empty() && st.top() != '(') {
                result += st.top();
                st.pop();
            }
            st.pop();
        }
        else {
            while (!st.empty() && precedence(st.top()) >= precedence(c)) {
                result += st.top();
                st.pop();
            }
            st.push(c);
        }
    }
    while (!st.empty()) {
        result += st.top();
        st.pop();
    }
    return result;
}

int main() {
    string infix = "A+(B*C)";
    cout << "Postfix: " << infixToPostfix(infix) << endl;
    return 0;
}
```

#### **Key Points:**
- Operators are pushed to the stack and only pop when needed.
- Parentheses **control precedence**.

#### **Memory Trick:**
- **"Operands go straight, operators wait!"**
- Always pop higher precedence operators **before pushing a new one**.

---

### **2.2 Postfix to Infix Conversion**
#### **Algorithm**:
1. Use a stack to store intermediate expressions.
2. Read the expression left to right:
   - If operand: **Push to stack**.
   - If operator: **Pop two operands, form an infix expression (`(A op B)`), and push back.**
3. At the end, stack contains the final expression.

#### **Code (C++)**:
```cpp
#include <bits/stdc++.h>
using namespace std;

string postfixToInfix(string s) {
    stack<string> st;
    for (char c : s) {
        if (isalnum(c))
            st.push(string(1, c));
        else {
            string a = st.top(); st.pop();
            string b = st.top(); st.pop();
            st.push("(" + b + c + a + ")");
        }
    }
    return st.top();
}

int main() {
    string postfix = "AB+C*";
    cout << "Infix: " << postfixToInfix(postfix) << endl;
    return 0;
}
```

#### **Memory Trick:**
- **"Operands first, operator in between!"**
- Pop two, apply the operator, and push back.

---

### **2.3 Prefix to Infix Conversion**
#### **Algorithm**:
1. Start from **right to left**.
2. If an operand appears, push it onto a stack.
3. If an operator appears, pop two operands, form an infix expression, and push back.
4. At the end, the stack contains the final infix expression.

#### **Code (C++)**:
```cpp
#include <bits/stdc++.h>
using namespace std;

string preToInfix(string s) {
    reverse(s.begin(), s.end());
    stack<string> st;
    for (char c : s) {
        if (isalnum(c))
            st.push(string(1, c));
        else {
            string a = st.top(); st.pop();
            string b = st.top(); st.pop();
            st.push("(" + a + c + b + ")");
        }
    }
    return st.top();
}

int main() {
    string prefix = "*+AB-CD";
    cout << "Infix: " << preToInfix(prefix) << endl;
    return 0;
}
```

#### **Memory Trick:**
- **"Right to left, same rule applies!"**
- Pop two, apply the operator, push back.

---

### **2.4 Infix to Prefix Conversion**
#### **Algorithm**:
1. Reverse the infix expression.
2. Replace `(` with `)` and vice versa.
3. Convert the new expression to postfix.
4. Reverse the obtained postfix expression to get the prefix.

#### **Memory Trick:**
- **"Reverse, swap, postfix, reverse!"**

---

### **2.5 Prefix to Postfix Conversion**
1. Read expression from **right to left**.
2. If operand: **Push to stack**.
3. If operator: **Pop two elements, combine them in postfix form, push back.**

#### **2.6 Postfix to Prefix Conversion**
1. Read expression from **left to right**.
2. If operand: **Push to stack**.
3. If operator: **Pop two elements, combine them in prefix form, push back.**

---

## **Final Comparison Table**
| **Conversion**       | **Algorithm**                                      | **Key Trick** |
|----------------------|----------------------------------------------------|--------------|
| Infix → Postfix     | Stack-based precedence                              | Operands go first! |
| Postfix → Infix     | Stack, pop two, insert operator in between         | Operands first, operator in between! |
| Prefix → Infix      | Stack, right to left, pop two, insert operator     | Right to left, same rule applies! |
| Infix → Prefix      | Reverse, swap `(`/`)`, convert to postfix, reverse | Reverse, swap, postfix, reverse! |
| Prefix → Postfix    | Stack, right to left, pop two, push back           | Right to left, postfix formation! |
| Postfix → Prefix    | Stack, left to right, pop two, push back           | Left to right, prefix formation! |

This structured approach will help you understand and **memorize** expression conversions easily! 🚀



# **Expression Conversion Notes**

## **1. Expression Types**
### **1.1 Infix Expression:**
- Operators are written between operands.
- Example: `A + B`
- **Easy to read but requires parentheses to specify precedence.**

### **1.2 Prefix Expression (Polish Notation):**
- Operators appear before operands.
- Example: `+ A B`
- **Eliminates the need for parentheses.**

### **1.3 Postfix Expression (Reverse Polish Notation - RPN):**
- Operators appear after operands.
- Example: `A B +`
- **Easier for computation (used in stack-based evaluation).**

---

## **2. Conversion Techniques**

### **2.1 Infix to Postfix Conversion**
#### **Code (C++)**:
```cpp
bool isop(char a){
    return (a=='/' || a=='*' || a=='+' || a=='-' || a=='^' || a=='%');
}

int prio(char a){
    if(a=='+' || a=='-') return 1;
    if(a=='*' || a=='/') return 2;
    if(a=='^') return 3;
    return -1;
}

string infixToPostfix(string& s) {
    string ans="";
    stack<char> st;
    int i=0;
    while(i<s.size()){
         if(isalnum(s[i])){
             ans += s[i];
         } 
         else if(s[i]=='('){
             st.push(s[i]);
         }
         else if(s[i]==')'){
             while(!st.empty() && st.top()!='('){
                 ans += st.top();
                 st.pop();
             }
             st.pop();
         }
         else{
            while(!st.empty() && prio(s[i]) <= prio(st.top())){
                ans += st.top();
                st.pop();
            }
            st.push(s[i]);
         }
        i++;      
    }
    while(!st.empty()){
         ans += st.top();
         st.pop();
     }
     return ans;
}
```
#### **Key Points:**
- Use a stack to handle operators and precedence.
- Operators pop when precedence is lower or equal.
- Parentheses help in priority management.

#### **Memory Trick:**
- **"Operands go straight, operators wait!"**

---

### **2.2 Postfix to Infix Conversion**
#### **Code (C++)**:
```cpp
string postToInfix(string s) {
    stack<string> st;
    for (char c : s) {
        if (isalnum(c)) {
            st.push(string(1, c));
        } else {
            string a = st.top(); st.pop();
            string b = st.top(); st.pop();
            st.push("(" + b + c + a + ")");
        }
    }
    return st.top();
}
```
#### **Key Points:**
- Read operands, push to stack.
- Pop two operands when an operator appears.
- Push back the formed expression.

#### **Memory Trick:**
- **"Operands first, operator in between!"**

---

### **2.3 Postfix to Prefix Conversion**
#### **Code (C++)**:
```cpp
string postToPre(string s) {
    stack<string> st;
    for (char c : s) {
        if (isalnum(c)) {
            st.push(string(1, c));
        } else {
            string a = st.top(); st.pop();
            string b = st.top(); st.pop();
            st.push(c + b + a);
        }
    }
    return st.top();
}
```
#### **Key Points:**
- Similar to Postfix to Infix, but **operators are placed first**.

#### **Memory Trick:**
- **"Reverse order of operations!"**

---

### **2.4 Prefix to Infix Conversion**
#### **Code (C++)**:
```cpp
string preToInfix(string s) {
    int i = s.size() - 1;
    stack<string> st;
    while(i >= 0) {
        if (isalnum(s[i])) {
            st.push(string(1, s[i]));
        } else {
            string a = st.top(); st.pop();
            string b = st.top(); st.pop();
            st.push("(" + a + s[i] + b + ")");
        }
        i--;
    }
    return st.top();
}
```
#### **Key Points:**
- **Process from right to left.**
- **Same rule as Postfix to Infix.**

#### **Memory Trick:**
- **"Right to left, insert operator in middle!"**

---

### **Final Comparison Table**
| **Conversion**       | **Algorithm**                                      | **Key Trick** |
|----------------------|----------------------------------------------------|--------------|
| Infix → Postfix     | Stack-based precedence                              | Operands go first! |
| Postfix → Infix     | Stack, pop two, insert operator in between         | Operands first, operator in between! |
| Prefix → Infix      | Stack, right to left, pop two, insert operator     | Right to left, insert operator in middle! |
| Infix → Prefix      | Reverse, swap `(`/`)`, convert to postfix, reverse | Reverse, swap, postfix, reverse! |
| Prefix → Postfix    | Stack, right to left, pop two, push back           | Right to left, postfix formation! |
| Postfix → Prefix    | Stack, left to right, pop two, push back           | Left to right, prefix formation! |

This structured approach will help you understand and **memorize** expression conversions easily! 🚀


