---
layout: post
title:  "C# WPF 103 TreeView"
categories: C#
---
# TreeView
### Intro
TreeView represents a control that displays hierarchical data in a tree structure that has items that can expand and collapse. The Explorer is an example of TreeView. <br>

TreeView will hold treeview item

# Example
Implement a frame show logical drive and files:

#### XML: using  a TreeView template to load resources
```r
<Window x:Class="WpfTreeView.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfTreeView"
        Loaded="Window_Loaded"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
       
    <StackPanel>
        
        <TreeView x:Name="FolderView">
            <TreeView.Resources>
                <Style TargetType="{x:Type TreeViewItem}">
                    <Setter Property="HeaderTemplate">
                        <Setter.Value>
                            <DataTemplate>
                                <StackPanel Orientation="Horizontal">
                                    <Image Margin="3" Width="20" Source="imgs/folderIcon.png"></Image>
                                    <TextBlock VerticalAlignment="Center" Text="{Binding}"></TextBlock>

                                </StackPanel>
                            </DataTemplate>
                        </Setter.Value>
                    </Setter>
                </Style>
            </TreeView.Resources>
        </TreeView>
    </StackPanel>
</Window>

```
#### C#: loading files and logical drives:
```r
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace WpfTreeView
{
    
    public partial class MainWindow : Window
    {
        #region Constructor
        public MainWindow()
        {
            InitializeComponent();
        }
        #endregion

        #region onload
        private void Window_Loaded(object sender, RoutedEventArgs e)
        {
            //Get all logical drive
            foreach (var drive in Directory.GetLogicalDrives())
            {
                //Create a Tree view item
                var item = new TreeViewItem()
                {
                    Header = drive,
                    Tag = drive
                };


                // Becasue Tree view is a tree, so it has its child 
                item.Items.Add(null);

                // Listen out for item being expanded
                item.Expanded += Folder_Expanded; 

                //add to the folderView
                FolderView.Items.Add(item);
            }
        }

        private void Folder_Expanded(object sender, RoutedEventArgs e)
        {
            var item = (TreeViewItem)sender;
            if(item.Items.Count != 1 || item.Items[0] != null)
            {
                return;
            }

            item.Items.Clear();

            var fullpath = (string)item.Tag;

            // Creat a blank list for directories
            var directories = new List<string>();
            try
            {
                var dirs = Directory.GetDirectories(fullpath);

                if(dirs.Length > 0)
                {
                    directories.AddRange(dirs);

                }
            }
            catch { }
            directories.ForEach(dirtoryPath =>
            {
                var subItem = new TreeViewItem()
                {
                    // set header as folder name
                    Header = GetFildFodlerName(dirtoryPath),
                    // and tag as full path
                    Tag = dirtoryPath
                };
                subItem.Items.Add(null);
                subItem.Expanded += Folder_Expanded;

                // expand the origin item
                item.Items.Add(subItem);
            });

        }

        /// <summary>
        /// File a file or folder name for a fullpath
        /// </summary>
        /// <param name="path"></param>
        public static string GetFildFodlerName(string path)
        {
            // C:\something\a folder

            if (string.IsNullOrEmpty(path))
            {
                return string.Empty;
            }
            // Handle both back slashes and forward slashes

            var normalizedPath = path.Replace('/','\\');
            var lastIndex = normalizedPath.LastIndexOf('\\');

            // if not slash, it self
            if (lastIndex <= 0)
            {
                return path;
            }

            // return name after the lash slash
            return normalizedPath.Substring(lastIndex);

        }

        #endregion
    }



}

```
#### Result:

![result](https://github.com/770120041/myblog/blob/master/images/2019-3/c%23_example_2.png?raw=true)