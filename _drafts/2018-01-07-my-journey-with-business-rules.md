---
layout: post
title: Recollection&#58; My journey with Business Rules
categories: [Programming]
tags: []
date: 2017-12-19 06:20:00 PM UTC
published: false
---

<!-- December 20, 2017 2:20:00 AM Philippine Time -->

In my five years of experience working as a programmer I have come into a deeper appreciation of the importance of separating the business rules from the other parts of a software system.

Since college I've been on search about how to best structure a software system.

I remember encountering this idea of 3-layers architecture in college --- Presentation layer, Business Logic layer (BLL), Data Access layer (DAL). But during that time I do not yet fully understand the _"why"_ part of the separation.

<!--more-->


But I remember using this 3-layers thing in one of my projects at school. What I remember doing in that project is placing the validation in the BLL, and placing all the SQL in the DAL.

_"I wonder what I really did in that project..."_

...So I tried to look for that project in my old files. Tada! I found it: ["StudInfoSys_VB.NETProj - 4.8"](insert link here)

There!... 

``` vb
Public Class StudentBLL
    ...
    Public Sub InsertStudent(ByVal FirstName As String, ...)
        ...
        If FirstName.Trim() = "" Then
            Throw New Exception("First Name is required")
        End If
        If LastName.Trim() = "" Then
            Throw New Exception("Last Name is required")
        End If
        
        studDAL.OpenConnection(connectionString)
        studDAL.InsertStudent(FirstName, ...)
        studDAL.CloseConnection()
    End Sub
    ...
End Class
```

``` vb 
Public Class StudentDAL
    ...
    Public Sub InsertStudent(ByVal FirstName As String, ...)
        Dim sqlstr As String = String.Format("INSERT INTO Students " & ...

        Using cmd As New OleDbCommand(sqlstr, Me.oledbConn)
            cmd.ExecuteNonQuery()
        End Using
    End Sub
    ...
End Class
```

`Modified: Thursday, ‎16 ‎February ‎2012, ‏‎11:43:22 PM`

_It was written in VB.NET?... in 2012?_

_Why?_

I thought I used C# in that project... Maybe it was a requirement at school to use VB.NET? I'm not sure... But maybe I just wanted to challenge myself whether I can still write in VB.NET during that time. Perhaps...

_But perhaps this project is not mine!?..._

I tried to look for some hints in the project so that I can be sure that this truly is mine...

``` vb
Public Class StudentBLL

...

'Created: February 17, 2012 7:00 PM to 2:00 AM the next day
```

_That sounds like me..._  :smile:

_And it truly is written in 2012!!_

But I'm sure I know this 3-layers thing before 2012! 

But I remember using it only in _one_ project...

_Hmmmm... Something must be wrong..._

Ahh... 

I found another project... created in 2010!...  it has a DAL but no BLL: ["StudInfoSys(TypedDataSets)"](insert link here)

<small>_("Look at that mess... 700 lines of code... I packed everything in one Form... and the presentation layer knows about DataSets!" :laughing:)_</small>


``` csharp
public partial class StudentForm : Form
{

// [in line 719]
    private void subjectIDComboBox_SelectedValueChanged(object sender, EventArgs e)
    {
        try
        {
            if (((ComboBox)sender).SelectedIndex == -1)
            {
                descriptiveTitleTextBox.Text = "";
                return;
            }

            string currSubjectID = subjectIDComboBox.Text;

            var subjects = from s in studInfoSysDataSet.Subjects
                            where s.SubjectID == currSubjectID
                            select s;

            //Descriptive title of currently selected SubjectID
            if (subjects != null)
            {
                descriptiveTitleTextBox.Text = subjects.Single().DescriptiveTitle;
            }
        }
        catch (InvalidOperationException)
        {
            this.regSubjectsBindingNavigatorPositionItem.Text = "NONE";
        }
    }
}
```


_"Why no BLL?"_

Because during that time, I really liked the drag-and-drop things that Visual Studio provides :laughing: :laughing: :laughing: ... where you just drag a GridView, change some properties in it to connect it to the database, and wa-la... you now have a working application.

In that kind of environment the presentation layer is tightly coupled to the data-access layer. Perhaps there was another way of doing it, but perhaps this was the only I know how to do it during that time.

Of course I later understood that these drag-and-drop things are evil... I mean, you would _not_ want to use it if you want your app to be manageable and scalable (or something like that).


