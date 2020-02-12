# 메뉴
```
<Window x:Class="WpfTutorialSamples.Common_interface_controls.MenuSample"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MenuSample" Height="200" Width="200">
    <DockPanel>
        <Menu DockPanel.Dock="Top">
            <MenuItem Header="_File">
                <MenuItem Header="_New" />
                <MenuItem Header="_Open" />
                <MenuItem Header="_Save" />
                <Separator />
                <MenuItem Header="_Exit" />
            </MenuItem>
        </Menu>
        <TextBox AcceptsReturn="True" />
    </DockPanel>
</Window>
```

# 로그인
```
    <Grid>

        <Grid.RowDefinitions>

            <RowDefinition Height="25*" />

            <RowDefinition Height="25*" />

            <RowDefinition Height="25*" />

            <RowDefinition Height="25*" />

        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>

            <ColumnDefinition Width="25*"/>

            <ColumnDefinition Width="25*"/>

            <ColumnDefinition Width="25*"/>

            <ColumnDefinition Width="25*"/>

        </Grid.ColumnDefinitions>

       

        <TextBlock Text="로그인 하세요" Grid.Row="0" Grid.ColumnSpan="4" FontSize="20" FontWeight="bold" HorizontalAlignment="Center" VerticalAlignment="Center" />

        <TextBlock Text="아이디" Grid.Row="1" Grid.Column="0" VerticalAlignment="Center" Margin="5" />

        <TextBox Grid.Row="1" Grid.Column="1" Grid.ColumnSpan="3" Margin="6" />

        <TextBlock Text="비밀번호" Grid.Row="2" Grid.Column="0" VerticalAlignment="Center" Margin="5" />

        <TextBox Grid.Row="2" Grid.Column="1" Grid.ColumnSpan="3" Margin="6" />

        <Button Content="확인" Grid.Row="3" Grid.Column="3" Margin="5" />

    </Grid>

</Window>

출처: https://dotnetmvp.tistory.com/28 [I will be a Programming Artist]
```
