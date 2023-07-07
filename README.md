# Windows_applications
using standard controls in Windows applications
#РОзмітка
<Window x:Class="Lab10.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Lab10"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <StackPanel>
            <Label>Виберіть спосіб обчислення</Label>
            <ComboBox Name="Type" Margin="5,20,0,30" ItemsSource="{ Binding Path=Options}" SelectedItem="{Binding Path=SelectedOption, Mode=TwoWay}">

            </ComboBox>
        </StackPanel>
        <StackPanel Grid.Row="1" Grid.Column="0" Margin="5,0,0,0">
            <Label>Введіть X</Label>
            <TextBox Padding="10,5,10,5" x:Name="X"/>
        </StackPanel>
        <StackPanel Grid.Row="1" Grid.Column="1" Margin="5,0,0,0">
            <Label>Введіть Y</Label>
            <TextBox Padding="10,5,10,5" x:Name="Y"/>
        </StackPanel>
        <StackPanel Grid.Row="1" Grid.Column="2" Margin="5,0,0,0">
            <Label>Введіть число ітерацій</Label>
            <TextBox Padding="10,5,10,5" x:Name="Count"/>
        </StackPanel>
        <StackPanel Grid.Row="2" Grid.Column="0" Margin="5,0,0,0">
            <Label>Введіть N</Label>
            <TextBox Padding="10,5,10,5" x:Name="N"/>
        </StackPanel>
        <StackPanel Grid.Row="2" Grid.Column="1" Margin="5,0,0,0">
            <Label>Введіть R</Label>
            <TextBox Padding="10,5,10,5" x:Name="R"/>
        </StackPanel>
        <StackPanel Grid.Row="3" Grid.Column="0" Margin="5,0,0,0">
            <Label>Введіть a</Label>
            <TextBox Padding="10,5,10,5" x:Name="A"/>
        </StackPanel>
        <StackPanel Grid.Row="3" Grid.Column="1" Margin="5,0,0,0">
            <Label>Введіть b</Label>
            <TextBox Padding="10,5,10,5" x:Name="B"/>
        </StackPanel>
        <StackPanel Grid.Row="3" Grid.Column="2" Margin="5,0,0,0">
            <Label>Введіть c</Label>
            <TextBox Padding="10,5,10,5" x:Name="C"/>
        </StackPanel>
        <StackPanel Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="3" Margin="5,30,0,0">
            <Button Click="Calculate_Click" Height="30" Width="200">Обчислити</Button>
        </StackPanel>
        <StackPanel Grid.Row="5" Margin="0,30,0,0">
            <Label>Результат</Label>
            <TextBlock x:Name="Result" Padding="10,0,10,0"></TextBlock>
        </StackPanel>
    </Grid>
</Window>
#Лістинг коду:

using System;
using System.Collections.Generic;
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

namespace Lab10
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    /// 

    public class ComboboxData
    {
        public string[] Options { get; set; } = new string[] { "1", "2" };
        public string SelectedOption { get; set; } = "1";
    }

    public partial class MainWindow : Window
    {
        public ComboboxData data { get; set; } = new ComboboxData();

        public MainWindow()
        {
            InitializeComponent();

            DataContext = data;
        }

        public double CalculateFirst()
        {
            double result = 0;
            double x = double.Parse(X.Text);
            double y = double.Parse(Y.Text);

            int prevI = 1;
            int j = 2;
            for (int i = 1; i < int.Parse(Count.Text); i += 2)
            {
                prevI *= j;
                double prev = Math.Pow(i % 3 == 0 ? x : y, i) / prevI;
                prevI *= j + 1;
                double next = Math.Pow(y, i + 1) / prevI;
                j += 2;
                result += -prev + next;
                if (Math.Abs(next - prev) < 0.0001)
                {
                    break;
                }
            }
            return result;
        }

        public double CalculateSecond()
        {
            double result = 0;
            int n = int.Parse(N.Text);
            int r = int.Parse(R.Text);
            double a = double.Parse(A.Text);
            double b = double.Parse(B.Text);
            double c = double.Parse(C.Text);

            for (int i = 1; i <= n; i ++)
            {
                for (int j = 1; j <= r; j++)
                {
                    result += (a * i + b * j) / (c * Math.Pow(i, j));
                }
            }
            return result;
        }

        private void Calculate_Click(object sender, RoutedEventArgs e)
        {
            double result = data.SelectedOption == "1" ? CalculateFirst() : CalculateSecond();
            Result.Text = result.ToString();

        }
    }
}


