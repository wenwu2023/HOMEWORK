@[TOC](Simple Java-based calculator in html format)
# 一级目录
## 二级目录
### 三级目录
# Introduction
This blog post shows steps on how to write a simple calculator in JavaScript. The project was compiled using vscode software. The language is JavaScript and the format is HTML.
# Link Table
| The Link Your Class | <https://bbs.csdn.net/forums/ssynkqtd-04> |
| -----------------  |--------------- |
| The Link of Requirement of This Assignment  | <https://bbs.csdn.net/topics/617332156> |
| The Aim of This Assignment | Calculator with a visual interface |
| MU STU ID and FZU STU ID | 21126585_832101101 |

Link to Github: 
# Problem-Solving Ideas
- The project needs to make a calculator with four basic operations and some advanced functions.
- The project will try to design a calculator to meet basic computing needs, so that functions can be added later.
- Find related articles from the CSDN community to learn the basic functions to use.

# The process of design and implementation
## Design flowchart
![Design idea](https://img-blog.csdnimg.cn/2b53148a699a4a9286c981a0e4f82c74.png)
# Interface Initialization
- Declarative HTML format, based on Javascript design program, using CSS to design an interactive table.

 # User Input
  * Directly Input
![Text](https://img-blog.csdnimg.cn/8d87129bab3547a483a6979e6e78d7fb.gif)

  * By Button
![Button](https://img-blog.csdnimg.cn/c6a2e9ee6145498e860ce333528d4753.gif)
_"onclick" will execute a function to input the numId._ 
# Calculation and Result in String Form
The program uses a string variable to record the calculation, so any button press should modify the string. After calculating the result of the expression using *eval()*, it is stored as the variable *result*. Then change the string value to result. This step allows the program to use a custom function *Ans()* to add the result of the previous calculation.
## Calculate with *eval()*
In the program, the address of the button and the display are numId, and the following numId is the address of the display.
```css
<td colspan="4">
<input type="text" id="numId" style="height: 90px; 
width: 350px; font-size: 50px" >
</td>
/* Calculator display in CSS table*/
```

```javascript
// The functions of Button "=" and "Ans"
var str;
var result=0;
function Press_Result()
        {
                    str=document.getElementById("numId");
                    // Read the contents of numId
                    result = eval(str.value).toFixed(7);
                    // The result retains 7 decimal places
                    str.value = result;
        }
function Ans(){
            str.value = str.value + result;
        }
```
## Use Math Library to Achieve Advanced Functionalities
Since we need to evaluate string values, we can use functions from the Math library to represent advanced functionalities. 
For example:

```css
/* The button "sin" */
<td>
<input type="button" value="sin" id="sin" 
onclick="Press_Sign('Math.sin(')"
style="height: 70px; width: 80px; font-size: 35px">
</td>
/* The button "cos" */
<td>
<input type="button" value="cos" id="cos" 
onclick="Press_Sign('Math.cos(')"
style="height: 70px; width: 80px; font-size: 35px">
</td>
```
## Design The Output Display According to Realistic Calculator Logic
In order for the final result to be as close as possible to the real calculator, we need the final output to be able to reflect errors or, in some cases, remove superfluous symbols. This aspect will be explained in special logic.

# Partial special logic
This program requires some special logic to reduce misinput and invalid results.
## Clear the Previous Result Display Before Starting A New Calculation
The logic here is to detect if the last input was "equal". Then we add this function to the event function of the number key and the special symbol.
```javascript
// The value to record 
var isequal = false;
function Press_Result()
{
           isequal = true;
           // ...
}
function check_equal(){
         if(isequal){
         C();
         isequal = false;
         }
}
function Press_Num(num)
        {
            check_equal();
           //...
        }
function Press_Sign(sign)
        {
            check_equal();
            // ...
        }
```
## Replace incorrect symbol input in some case
Three scenarios are considered here:
- The last output before pressing "=" is a symbol (get invalid output);
- String The first input is a symbol (ibid.)
- Enter a symbol followed by another symbol
```javascript
var issign = false;
function get_sign(){
            issign = true;
        }
function Delete_sign(){
            if(issign){
                Press_Delete();
                // Delete an invalid symbol
                // Only one-character symbols can be output
                // It needs improvement
            }
        }
function Press_Sign(sign)
        {
            check_equal();
            str = document.getElementById("numId");
            Delete_sign();
            if(str.value == ""){
                str.value = str.value + "0";
            }
            // It means the string should start with 0 
            // but not a symbol
            str.value = str.value + sign;
            get_sign();
        }
function Press_Result()
        {
                    isequal = true;
                    // issign=true if there has a signed output
                    Delete_sign();
                   // ...
        }
```
![Symbolic problem handling](https://img-blog.csdnimg.cn/a55f86295b6948a6bf8def4c520d60d5.gif)
## Error Expression Results In "error"
Just use "try{}catch{}"

```javascript
function Press_Result()
        {
                    isequal = true;
                    Delete_sign();
                    str=document.getElementById("numId");
                    try{
                        result = eval(str.value).toFixed(7);
                        str.value = result;
                    }catch{
                        str.value="error";
                    }
        }
```
![Error](https://img-blog.csdnimg.cn/061e9d7a4bfd4f6a89b6713ec6471e02.gif)
Code
JAVASCRIPT

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>JAVA-CAL</title>
    <h1>Made by 1101</h1>
    <h3>The initial Ans is none</h3>
    <h3>Follow the logic of parentheses</h3>
    <script type="text/javascript">

        var str;
        var result=0;
        var isequal = false;
        var issign = false;

        function get_sign(){
            issign = true;
        }
        function Delete_sign(){
            if(issign){
                Press_Delete();
            }
        }
        function C()
        {
            str = document.getElementById("numId");
            str.value = "";
        }
        function Press_Delete(){
            str = document.getElementById("numId");
            var length=str.value.length;
            str.value = str.value.substr(0,length-1);
        }
        function check_equal(){
            if(isequal){
                C();
                isequal = false;
            }
        }
        function Press_Sign(sign)
        {
            check_equal();
            str = document.getElementById("numId");
            Delete_sign();
            if(str.value == ""){
                str.value = str.value + "0";
            }
            str.value = str.value + sign;
            get_sign();
        }
        function Press_Num(num)
        {
            check_equal();
            issign=false;
            str = document.getElementById("numId");
            str.value = str.value + num;
        }
        function Ans(){
            check_equal();
            str.value = str.value + result;
        }
        function Press_Result()
        {
                    isequal = true;
                    Delete_sign();
                    str=document.getElementById("numId");
                    try{
                        result = eval(str.value).toFixed(7);
                        str.value = result;
                    }catch{
                        str.value="error";
                    }
        }
    </script>
```
CSS TABLE

```css
 </head>
 <body bgcolor="009966">
        <table border="8px" align="center" bgColor="808080"
               style="height: 400px; width: 460px">  
            <tr>
                <td colspan="4">
                    <input type="text" id="numId" style="height: 90px; width: 350px; font-size: 50px" >
                </td>
            </tr>
            <tr>
                <td>
                    <input type="button" value="C" id="C" onclick="C()"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="sin" id="sin" onclick="Press_Num('Math.sin(')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="cos" id="cos" onclick="Press_Num('Math.cos(')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="^" id="^" onclick="Press_Sign('**')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="√" id="√" onclick="Press_Num('Math.sqrt(')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
            </tr>
            <tr>
                <td>
                    <input type="button" value="7" id="7" onclick="Press_Num(7)"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="8" id="8" onclick="Press_Num(8)"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="9" id="9" onclick="Press_Num(9)"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="+" id="add" onclick="Press_Sign('+')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="%" id="%" onclick="Press_Num('*0.01')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
            </tr>
            <tr>
                <td>
                    <input type="button" value="4" id="4" onclick="Press_Num(4)"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="5" id="5" onclick="Press_Num(5)"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="6" id="6" onclick="Press_Num(6)"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="-" id="sub" onclick="Press_Sign('-')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="tan" id="tan" onclick="Press_Num('Math.tan(')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
            </tr>
            <tr>
                <td>
                    <input type="button" value="1" id="1" onclick="Press_Num(1)"
                            style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="2" id="2" onclick="Press_Num(2)"
                             style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="3" id="3" onclick="Press_Num(3)"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="*" id="mul" onclick="Press_Sign('*')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="^2" id="^2" onclick="Press_Num('**2.0')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
            </tr>
            <tr>
                <td>
                    <input type="button" value="0" id="0" onclick="Press_Num(0)"
                            style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="Ans" id="Ans" onclick="Ans()"
                            style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="." id="point" onclick="Press_Sign('.')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="/" id="divide" onclick="Press_Sign('/')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="e" id="e" onclick="Press_Num('Math.E')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
            </tr>
            <tr>
                <td>
                    <input type="button" value="Del" id="clear" onclick="Press_Delete()"
                             style="height: 70px; width: 80px; font-size: 35px" >
                </td>
                <td>
                    <input type="button" value="=" id="result" onclick="Press_Result()"
                             style="height: 70px; width: 80px; font-size: 35px" >
                </td>
                <td>
                    <input type="button" value="(" id="point" onclick="Press_Num('(')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value=")" id="divide" onclick="Press_Num(')')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
                <td>
                    <input type="button" value="pi" id="pi" onclick="Press_Num('Math.PI')"
                           style="height: 70px; width: 80px; font-size: 35px">
                </td>
            </tr>
        </table>
    </body>
    </html>
```
Simple Process
![Process](https://img-blog.csdnimg.cn/b6a11c6374524d9d96798c8ab4fe5667.png)
#  Result Display
![Display](https://img-blog.csdnimg.cn/384be98608464fb6bf01824df49885e7.gif)
# Summary
- The program can basically complete the daily basic operations, and has 7 decimal precision.
- The program also needs more functions to complete the logic, and it needs to integrate and simplify parts of the code in the future.

