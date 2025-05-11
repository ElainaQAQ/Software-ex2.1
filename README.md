Android Jetpack Compose基础实践

Compose 入门

Android 使用@Composable注解来表示可组合函数，从而定义UI。使用Compose时，Activity 仍然是 Android 应用的入口点。在Compose中，setContent 来定义布局，但不同于在传统 View 系统中使用 XML 文件，将在该函数中调用可组合函数。此外，若要使用 Android Studio 预览，只需使用 @Preview 注解标记所有无参数可组合函数或采用默认形参的函数，然后构建项目即可。

微调界面

为Greeting 设置不同的背景色，使用Surface 包围 Text 可组合项。Surface 会采用一种颜色，因此请使用 MaterialTheme.colorScheme.primary。

@Composable

fun Greeting(name: String, modifier: Modifier = Modifier) {

    Surface(color = MaterialTheme.colorScheme.primary) {
    
        Text(
        
            text = "Hello $name!",
            
            modifier = modifier
            
        )
        
    }
    
}

![4049d5f5e93ad0b4504cc8a19209999](https://github.com/user-attachments/assets/7fd19aa9-df20-45c0-8ced-ce35ee0fb32e)

修饰符

大多数 Compose 界面元素（例如 Surface 和 Text）都接受可选的 modifier 参数。修饰符会指示界面元素如何在其父布局中放置、显示或表现。Greeting 可组合项已有一个默认修饰符，该修饰符随后会传递给 Text。

可以试着修改 modifier 的属性，例如，padding 修饰符会在其修饰的元素周围应用一定的空间。

import androidx.compose.foundation.layout.padding

import androidx.compose.ui.unit.dp

// ...


@Composable

fun Greeting(name: String, modifier: Modifier = Modifier) {

    Surface(color = MaterialTheme.colorScheme.primary) {
    
        Text(
        
            text = "Hello $name!",
            
            modifier = modifier.padding(24.dp)
            
        )
        
    }
    
}

重复使用可组合项

创建一个MyApp 的可组合项，该组合项中包含Greeting。

@Composable

fun MyApp(modifier: Modifier = Modifier) {

    Surface(
    
        modifier = modifier,
        
        color = MaterialTheme.colorScheme.background
        
    ) {
    
        Greeting("Android")
        
    }
    
}

创建Compose中的列（Column）和行（Row）

import androidx.compose.foundation.layout.Column

// ...

@Composable

fun Greeting(name: String, modifier: Modifier = Modifier) {

    Surface(color = MaterialTheme.colorScheme.primary) {
    
        Column(modifier = modifier.padding(24.dp)) {
        
            Text(text = "Hello ")
            
            Text(text = name)
            
        }
        
    }
    
}


Compose 和 Kotlin

可组合函数可以像 Kotlin 中的其他函数一样使用。这会使界面构建变得非常有效，可以添加语句来影响界面的显示方式。

例如，修改之前定义的MyApp

@Composable

fun MyApp(

    modifier: Modifier = Modifier,
    
    names: List<String> = listOf("World", "Compose")
    
) {

    Column(modifier) {
    
        for (name in names) {
        
            Greeting(name = name)
            
        }
        
    }
    
}

![ec9c27b99f8118a277f53bdbad59f5b](https://github.com/user-attachments/assets/ce491d52-e115-4372-acdf-e7f6130fc018)

当前尚未设置可组合项的尺寸，也未对可组合项的大小添加任何限制，因此每一行仅占用可能的最小空间，预览时的效果也是如此。更改预览效果，以模拟小屏幕手机的常见宽度 320dp。按如下所示向 @Preview 注解添加 widthDp 参数：

@Preview(showBackground = true, widthDp = 320)

@Composable

fun GreetingPreview() {

    BasicsCodelabTheme {
    
        MyApp()
        
    }
    
}

![60932cbbeadce395d5331bb08b5a487](https://github.com/user-attachments/assets/5f81aed2-2945-4659-9acb-1c73d29528fb)

接下来为修饰符（modifier）添加更多的属性，来查看显示效果。使用 fillMaxWidth 和 padding 修饰符复制以下布局。注意更改工程中的代码，使用以下代码修改Compose函数。

import androidx.compose.foundation.layout.fillMaxWidth

@Composable

fun MyApp(

    modifier: Modifier = Modifier,
    
    names: List<String> = listOf("World", "Compose")
    
) {

    Column(modifier = modifier.padding(vertical = 4.dp)) {
    
        for (name in names) {
        
            Greeting(name = name)
            
        }
        
    }
    
}

@Composable

fun Greeting(name: String, modifier: Modifier = Modifier) {

    Surface(
    
        color = MaterialTheme.colorScheme.primary,
        
        modifier = modifier.padding(vertical = 4.dp, horizontal = 8.dp)
        
    ) {
    
        Column(modifier = Modifier.fillMaxWidth().padding(24.dp)) {
        
            Text(text = "Hello ")
            
            Text(text = name)
            
        }
        
    }
    
}

![61e78dc00b1e92e823ef9d199aa62d1](https://github.com/user-attachments/assets/702d42e8-289d-4094-8109-950af237002b)

添加按钮

import androidx.compose.foundation.layout.Row

import androidx.compose.material3.ElevatedButton

// ...


@Composable

fun Greeting(name: String, modifier: Modifier = Modifier) {

    Surface(
    
        color = MaterialTheme.colorScheme.primary,
        
        modifier = modifier.padding(vertical = 4.dp, horizontal = 8.dp)
        
    ) {
    
        Row(modifier = Modifier.padding(24.dp)) {
        
            Column(modifier = Modifier.weight(1f)) {
            
                Text(text = "Hello ")
                
                Text(text = name)
                
            }
            
            ElevatedButton(
            
                onClick = { /* TODO */ }
                
            ) {
            
                Text("Show more")
                
            }
            
        }
        
    }
    
}

![68cf1fda9905fc3081c9b2d844006f9](https://github.com/user-attachments/assets/1a39269b-f1b4-4c59-a528-f43b17606640)

Compose中的状态（State）

@Composable

fun Greeting(name: String, modifier: Modifier = Modifier) {

    val expanded = remember { mutableStateOf(false) }
    
    val extraPadding = if (expanded.value) 48.dp else 0.dp
    
    Surface(
    
        color = MaterialTheme.colorScheme.primary,
        
        modifier = modifier.padding(vertical = 4.dp, horizontal = 8.dp)
        
    ) {
    
        Row(modifier = Modifier.padding(24.dp)) {
        
            Column(
            
                modifier = Modifier
                
                    .weight(1f)
                    
                    .padding(bottom = extraPadding)
                    
            ) {
            
                Text(text = "Hello ")
                
                Text(text = name)
                
            }
            
            ElevatedButton(
            
                onClick = { expanded.value = !expanded.value }
                
            ) {
            
                Text(if (expanded.value) "Show less" else "Show more")
                
            }
            
        }
        
    }
    
}

![af3ccaf3179cd28e8c4729896a5fb66](https://github.com/user-attachments/assets/37f8d3c4-f5ca-4a70-bdde-6baed58bdaa2)


