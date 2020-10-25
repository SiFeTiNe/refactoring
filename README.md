# refactoring
## Calculator
From my Calculator repository (C#): https://github.com/keyboard2543/Calculator

In `/Calculator/Form1.cs` at the top of code:<br>
>Unusing imported package

Before refactoring: 
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Runtime.InteropServices;

...
```
Delete unusing imported package</br>
After refactoring: 
```
using System;
using System.Linq;
using System.Windows.Forms;

...
```
---

In `/Calculator/Form1.cs`
>Repeated code

Before refactoring:
```
        ...

        private void pictureBox20_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length == 0)
            {
                textBox2.Text = textBox1.Text;
                textBox1.Clear();
            }
            if (textBox2.Text.Length != 0)
                labelOperator.Text = percentText;
        }

        private void pictureBox13_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length == 0)
            {
                textBox2.Text = textBox1.Text;
                textBox1.Clear();
            }
            if (textBox2.Text.Length != 0)
                labelOperator.Text = plusText;
        }

        private void pictureBox14_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length == 0)
            {
                textBox2.Text = textBox1.Text;
                textBox1.Clear();
            }
            if (textBox2.Text.Length != 0)
                labelOperator.Text = minusText;
        }

        private void pictureBox15_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length == 0)
            {
                textBox2.Text = textBox1.Text;
                textBox1.Clear();
            }
            if (textBox2.Text.Length != 0)
                labelOperator.Text = mulText;
        }

        private void pictureBox16_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length == 0)
            {
                textBox2.Text = textBox1.Text;
                textBox1.Clear();
            }
            if (textBox2.Text.Length != 0)
                labelOperator.Text = divText;
        }

        ...

        private void pictureBox21_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length != 0)
            {
                textBox1.Text = textBox2.Text;
                textBox2.Clear();
            }
            if (textBox1.Text.Length != 0)
                labelOperator.Text = sinText;
        }

        private void pictureBox22_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length != 0)
            {
                textBox1.Text = textBox2.Text;
                textBox2.Clear();
            }
            if (textBox1.Text.Length != 0)
                labelOperator.Text = cosText;
        }

        private void pictureBox23_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length != 0)
            {
                textBox1.Text = textBox2.Text;
                textBox2.Clear();
            }
            if (textBox1.Text.Length != 0)
                labelOperator.Text = tanText;
        }

        private void pictureBox25_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length != 0)
            {
                textBox1.Text = textBox2.Text;
                textBox2.Clear();
            }
            if (textBox1.Text.Length != 0)
                labelOperator.Text = sqrtText;
        }

        private void pictureBox26_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length == 0)
            {
                textBox2.Text = textBox1.Text;
                textBox1.Clear();
            }
            if (textBox2.Text.Length != 0)
                labelOperator.Text = rootText;
        }

        private void pictureBox27_Click(object sender, EventArgs e)
        {
            if (textBox2.Text.Length != 0)
            {
                textBox1.Text = textBox2.Text;
                textBox2.Clear();
            }
            if (textBox1.Text.Length != 0)
                labelOperator.Text = pow2Text;
        }
```
Extract `private void Operate(string textOperatorToOperate)` methods</br>
After refactoring: 
```
    ...

        private void Operate(string textOperatorToOperate)
        {
            if (textBox2.Text.Length != 0)
            {
                textBox1.Text = textBox2.Text;
                textBox2.Clear();
            }
            else labelOperator.Text = textOperatorToOperate;
        }

        ...

        private void pictureBox20_Click(object sender, EventArgs e)
        {
            Operate(percentText);
        }

        private void pictureBox13_Click(object sender, EventArgs e)
        {
            Operate(plusText);
        }

        private void pictureBox14_Click(object sender, EventArgs e)
        {
            Operate(minusText);
        }

        ...
```
---
In `/Calculator/Form1.cs`</br>
At `attributes initialization`</br>
>Constant string attributes are not constants
```
    ...

    public partial class Form1 : Form
    {
        string plusText = "+";
        string minusText = "-";
        string mulText = "x";
        string divText = "÷";
        string sqrtText = "√";
        string rootText = "√n";
        string pow2Text = "^2";
        string powText = "^";
        string sinText = "Sin";
        string cosText = "Cos";
        string tanText = "Tan";
        string percentText = "%";

        ...
    }
```
Make them to be constants</br>
After refactoring: 
```
    ...

    public partial class Form1 : Form
    {
        const string plusText = "+";
        const string minusText = "-";
        const string mulText = "x";
        const string divText = "÷";
        const string sqrtText = "√";
        const string rootText = "√n";
        const string pow2Text = "^2";
        const string powText = "^";
        const string sinText = "Sin";
        const string cosText = "Cos";
        const string tanText = "Tan";
        const string percentText = "%";

        ...
    }
```
---

In `/Calculator/Form1.cs`</br>
At `private void pictureBox12_Click(object sender, EventArgs e)` method</br>
>String equality are checking with `==`
```
        ...

        private void pictureBox12_Click(object sender, EventArgs e)
        {
            if (textBox1.Text.Length != 0 && labelOperator.Text.Length != 0)
            {
                if (labelOperator.Text == plusText)
                {
                    textBox1.Text = (double.Parse(textBox2.Text) + double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == minusText)
                {
                    textBox1.Text = (double.Parse(textBox2.Text) - double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == mulText)
                {
                    textBox1.Text = (double.Parse(textBox2.Text) * double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == divText)
                {
                    textBox1.Text = (double.Parse(textBox2.Text) / double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == sqrtText)
                {
                    textBox1.Text = Math.Sqrt(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == rootText)
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox2.Text), 1 / double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == pow2Text)
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox1.Text), 2).ToString();
                }
                else if (labelOperator.Text == powText)
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox2.Text), double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == sinText)
                {
                    textBox1.Text = Math.Sin(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == cosText)
                {
                    textBox1.Text = Math.Cos(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == tanText)
                {
                    textBox1.Text = Math.Tan(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text == percentText)
                {
                    textBox1.Text = (double.Parse(textBox1.Text) * double.Parse(textBox2.Text) / 100).ToString();
                }
                textBox2.Clear();
                labelOperator.Text = "";
            }
        }

        ...
```
Use Equals method</br>
After refactoring:
```
        ...

        private void pictureBox12_Click(object sender, EventArgs e)
        {
            if (textBox1.Text.Length != 0 && labelOperator.Text.Length != 0)
            {
                if (labelOperator.Text.Equals(plusText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) + double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(minusText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) - double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(mulText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) * double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(divText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) / double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(sqrtText))
                {
                    textBox1.Text = Math.Sqrt(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(rootText))
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox2.Text), 1 / double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(pow2Text))
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox1.Text), 2).ToString();
                }
                else if (labelOperator.Text.Equals(powText))
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox2.Text), double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(sinText))
                {
                    textBox1.Text = Math.Sin(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(cosText))
                {
                    textBox1.Text = Math.Cos(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(tanText))
                {
                    textBox1.Text = Math.Tan(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(percentText))
                {
                    textBox1.Text = (double.Parse(textBox1.Text) * double.Parse(textBox2.Text) / 100).ToString();
                }
                textBox2.Clear();
                labelOperator.Text = "";
            }
        }

        ...
```
---

In `/Calculator/Form1.cs`</br>
At `private void pictureBox12_Click(object sender, EventArgs e)` method
>Many of if statement using
```
        ...

        private void pictureBox12_Click(object sender, EventArgs e)
        {
            if (textBox1.Text.Length != 0 && labelOperator.Text.Length != 0)
            {
                if (labelOperator.Text.Equals(plusText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) + double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(minusText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) - double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(mulText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) * double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(divText))
                {
                    textBox1.Text = (double.Parse(textBox2.Text) / double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(sqrtText))
                {
                    textBox1.Text = Math.Sqrt(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(rootText))
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox2.Text), 1 / double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(pow2Text))
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox1.Text), 2).ToString();
                }
                else if (labelOperator.Text.Equals(powText))
                {
                    textBox1.Text = Math.Pow(double.Parse(textBox2.Text), double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(sinText))
                {
                    textBox1.Text = Math.Sin(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(cosText))
                {
                    textBox1.Text = Math.Cos(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(tanText))
                {
                    textBox1.Text = Math.Tan(double.Parse(textBox1.Text)).ToString();
                }
                else if (labelOperator.Text.Equals(percentText))
                {
                    textBox1.Text = (double.Parse(textBox1.Text) * double.Parse(textBox2.Text) / 100).ToString();
                }
                textBox2.Clear();
                labelOperator.Text = "";
            }
        }

        ...
```
Use switch...case</br>
After refactoring: 
```
        ...

        private void pictureBox12_Click(object sender, EventArgs e)
        {
            if (textBox1.Text.Length != 0 && labelOperator.Text.Length != 0)
            {
                switch(labelOperator.Text)
                {
                    case plusText:
                        textBox1.Text = (double.Parse(textBox2.Text) + double.Parse(textBox1.Text)).ToString();
                        break;
                    case minusText:
                        textBox1.Text = (double.Parse(textBox2.Text) - double.Parse(textBox1.Text)).ToString();
                        break;
                    case mulText:
                        textBox1.Text = (double.Parse(textBox2.Text) * double.Parse(textBox1.Text)).ToString();
                        break;
                    case divText:
                        textBox1.Text = (double.Parse(textBox2.Text) / double.Parse(textBox1.Text)).ToString();
                        break;
                    case sqrtText:
                        textBox1.Text = Math.Sqrt(double.Parse(textBox1.Text)).ToString();
                        break;
                    case rootText:
                        textBox1.Text = Math.Pow(double.Parse(textBox2.Text), 1 / double.Parse(textBox1.Text)).ToString();
                        break;
                    case pow2Text:
                        textBox1.Text = Math.Pow(double.Parse(textBox1.Text), 2).ToString();
                        break;
                    case powText:
                        textBox1.Text = Math.Pow(double.Parse(textBox2.Text), double.Parse(textBox1.Text)).ToString();
                        break;
                    case sinText:
                        textBox1.Text = Math.Sin(double.Parse(textBox1.Text)).ToString();
                        break;
                    case cosText:
                        textBox1.Text = Math.Cos(double.Parse(textBox1.Text)).ToString();
                        break;
                    case tanText:
                        textBox1.Text = Math.Tan(double.Parse(textBox1.Text)).ToString();
                        break;
                    case percentText:
                        textBox1.Text = (double.Parse(textBox1.Text) * double.Parse(textBox2.Text) / 100).ToString();
                        break;
                    default:
                        textBox1.Text = "No operator to oprerate";
                        break;
                };
                textBox2.Clear();
                labelOperator.Text = "";
            }
        }

        ...
```