---
layout: post
title: Android 4���� ���� ����
date: 2021-10-12 23:41
description: 
author: leeeha
categories: ["android"]
---

�ȳ��ϼ���. ���� GDSC �ȵ���̵� ��Ʈ ��� �������Դϴ�. �̹� 4���� ���ǿ��� ������ ������ �����غ��ڽ��ϴ�. ���뵵 ���� �ʰ� �����ϴٺ��� ���� ����� �� ������ ������ �� ������ּ���! �׸��� �߸� ������ �κ��� �ִٸ� ��۷� �� �˷��ֽñ� �ٶ��ϴ�!

<br>

![image](https://user-images.githubusercontent.com/68090939/136978265-709bf3c1-26f0-42ad-abee-702315e21581.png)

����� 1������ Jetpack Compose�� ���� UI�� �ǽ��ϰ�, 2������ state�� ���� ���ظ� �������� CheggPrep ���� ���� ����� �����غý��ϴ�. �׸��� ���� 3�������� ���� 4���� ȭ���� ��������ϴ�.<br>

1. ī�װ����� �÷���ī�� ����� �����ִ� HomeScreen
2. �÷���ī�� �˻��� ���� SearchScreen
3. ���ο� �÷���ī�� ������ ���� CreateScreen
4. �߰� ������ �����ִ� MoreScreen

<br>

�̹� 4������ ������ ������ ũ�� 3�����Դϴ�.
1. ProgressBar�� �̿��� ���� ī�尡 ���° ī������ �����ֱ�
2. ���� ȭ��� ���� ��ȯ�� �����ϰ� ���ִ� Navigation�� ���� ���� �� ����
3. �÷���ī���� ������ �����ִ� DeckScreen �߰��ϱ�

<hr>

# 1. ProgressBar

![image](https://user-images.githubusercontent.com/68090939/136987136-a675d38c-fffc-4832-ac9f-d1cf8cf34b79.png)
<br>

�� ������ ���� ���� ����� ���α׷����ٸ� �����Ϸ���, LinearProgressIndicator�� progress ���ڿ� ��ư Ŭ���� ���� tween �ִϸ��̼��� �������ָ� �˴ϴ�. ��ü �ڵ�� ������ �����ϴ�.

```kotlin
@Composable
fun MainScreen() {
    val (count, setCount) = remember { // ���� ī�� ��
        mutableStateOf(0f)
    }

    val totalCount = 7 // �� ī�� ��

    LaunchedEffect(key1 = true){ // �Ʒ� �ο� ���� ����
        setCount(1f)
    }

    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        modifier = Modifier.padding(16.dp)
    ){
        // TopBar�� �� �ؽ�Ʈ
        Text(
            text = "${count.toInt()} / $totalCount",
            fontWeight = FontWeight.Bold
        )

        Spacer(modifier = Modifier.height(16.dp))

        // ProgressBar
        MyProgressBar(count = count, totalCount = totalCount)

        Spacer(modifier = Modifier.height(16.dp))

        // ��ư�� ������ ProgressBar ���°� �ٲ�鼭 ī�尡 �Ѿ��.
        Row(
            Modifier
                .padding(horizontal = 16.dp)
                .fillMaxWidth(),
            horizontalArrangement = Arrangement.SpaceEvenly
        ) {
            Button(
                onClick = { if (count > 1) setCount(count - 1) }
            ){
                Text("���� ī��")
            }

            Button(
                onClick = { if (count < totalCount) setCount(count + 1) }
            ){
                Text("���� ī��")
            }
        }
    }
}

@Composable
fun MyProgressBar(
    modifier: Modifier = Modifier,
    color: Color = DeepOrange,
    animDuration: Int = 300,
    animDelay: Int = 0,
    count: Float,
    totalCount: Int,
) {
    val curPercentage by animateFloatAsState(
        targetValue = count / totalCount,
        animationSpec = tween(
            durationMillis = animDuration,
            delayMillis = animDelay,
            easing = LinearOutSlowInEasing // �Ʒ� �ο� ���� ����
        )
    )

    LinearProgressIndicator(
        modifier = Modifier
            .fillMaxWidth()
            .height(12.dp)
            .clip(CircleShape),
        progress = curPercentage,
        color = color,
        backgroundColor = Color.LightGray
    )
}
```
<br>

**�ڵ� �ο� ����** (LaunchedEffect, LinearOutSlowInEasing)

<br>

```kotlin
 LaunchedEffect(key1 = true){
    setCount(1f)
}
```

���⼭ LaunchedEffect�� ó���� progress bar�� ���¸� 0���� 1�� ������ų �� setCount(1f)�� ����� ������ �� �ְ� ���ݴϴ�. launched effect ���״�� �ִϸ��̼��� ���۵� �� ����Ǵ� ȿ���ε�, �����ϰ� ���¸� ������ �� �ְ� ���ִ� composable�̶�� �����ϸ� �˴ϴ�.<br>

[https://developer.android.com/jetpack/compose/side-effects#launchedeffect](https://developer.android.com/jetpack/compose/side-effects#launchedeffect)<br>

�� ���� ������ ������ �ڷ�ƾ, suspend �Լ��� ������ ������ �ð��� �� �ٽ� �����غ��߰ڽ��ϴ�,,

<br>

```kotlin
val curPercentage by animateFloatAsState(
    targetValue = count / totalCount,
    animationSpec = tween(
        durationMillis = animDuration,
        delayMillis = animDelay,
        easing = LinearOutSlowInEasing // �Ʒ� �ο� ���� ����
    )
)
```

[���� ��ũ](https://medium.com/@Kjoon/%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98-%EB%94%94%EC%9E%90%EC%9D%B8-%EC%9D%B4%EC%95%BC%EA%B8%B0-2-easing-functions-cf0f6cb213a2)<br>

easing curve�� �������� ���� ��� �˻��غý��ϴ�. �ִϸ��̼ǿ��� ���۰� ���� �������� �������ִ� ���̶�� �����ϸ� �� �� �����ϴ�.<br>

<hr>
<br>

# 2. Navigation

## Navigation�� ����� �������

[https://developer.android.com/guide/navigation](https://developer.android.com/guide/navigation)

Navigation�� ����ڰ� �� ���� �پ��� �������� Ž���ϰ�, ����, �ڷ� �̵��� �� ���Ǵ� �����Դϴ�. ��, ���� ȭ�� ���� ��ȯ�� �����ϰ� ���ִ� ���̶�� �����ϸ� �˴ϴ�.<br>

Navigation���� ũ�� 3���� ���� ��Ұ� �ֽ��ϴ�. <br>

**1. Navigation graph**

navigation�� ���õ� ��� ������ ���ԵǾ� �ִ� XML ���ҽ��Դϴ�. ���� ���⿡�� **navigation�� ������(destination), �� �� ������ �̵� ������ ��� ȭ���**�� ���Ե˴ϴ�. ���� �׷����� �ᱹ a, b, c, d ȭ�� ���� ��ȯ�� ��Ÿ�� ���Դϴ�.<br>

![image](https://user-images.githubusercontent.com/68090939/137103343-4d24adc8-7f4e-4595-8aa1-42a245ffe825.png)

<br>

**2. NavHost**

**�׺���̼� �׷����� �������� ��� �����̳��Դϴ�. ��, �� ������ �̵��� ��� ȭ����� �� �����̳ʿ� ���� ������ ���̰� �˴ϴ�.** �׺���̼� ������ҿ��� NavHost�� �⺻ �������μ�, NavHostFragment�� ���ԵǾ� �ֽ��ϴ�. ���� NavHost�� **fragment���� ���� �� �ڸ�**��� �� �� �ֽ��ϴ�. (fragment�� �κ�, �����̶�� ������? ���״�� �����׸�Ʈ�� ��Ƽ��Ƽ�� ������ ���� "�κ� ȭ��"���μ�, UI�� ���� ���� ��� ������ �ۼ��� �� �ְ� ���ݴϴ�.) <br>

��  ����: ���� ����� composable�� ȭ���� ����� ������, �׷����� composable������ destination���� �����ϰ� �ֱ� ������ NavHost�� composable�� ���� �ڸ���� ǥ���ϴ� �� �� ��Ȯ�մϴ�!<br>

<img src="https://user-images.githubusercontent.com/68090939/137103507-a83cafc4-bb12-4564-9eca-7663844dd019.png" width="450px"/><br>

<br>

**3. NavController**

**NavHost������ �׺���̼��� �����ϴ� ��ü�Դϴ�. NavController�� ����ڰ� �� ��ü�� �̵��� ��, NavHost �ڸ��� �����׸�Ʈ���� �鿩�����ų� �������鼭, ���� ȭ�� ���� ��ȯ�� �̷���� �� �ְ� ���ݴϴ�.** 
���� Ž���� �� �׺���̼� �׷����� Ư�� ��θ� ���� Ž���ϰų�, Ư�� ������� ���� Ž���ϰ� �ʹٰ� NavController���� �˸���, NavController�� �ش� destination�� NavHost�� �����ϰ� �˴ϴ�.<br>

- destination - navigation�� ������, �̵��� ��� ȭ��
- route�� ���� navigate (�ٸ� ������ ȭ������ �̵�)
- BackStackEntry - navigation�� ���� ����ϴ� ����

<hr>
<br>

## Navigation�� ����

�������� ��α׿� ���� ������ �� �صμ̱� ������, ������ ����Ʈ�� �б� ���� �� �� ���� ��ũ���� �� �⺻ ������ ���� �����ϰ� ���ñ� �����մϴ�!<br>

[https://blog.naver.com/comye1/222457844226](https://blog.naver.com/comye1/222457844226)<br>
[https://blog.naver.com/comye1/222482497797](https://blog.naver.com/comye1/222482497797)

<br>

����, app ������ build.gradle�� ���� �ڵ带 �߰����ݴϴ�.<br>

`implementation "androidx.navigation:navigation-compose:2.4.0-alpha05"`

<br>

**1. navController�� ������ ����Ѵ�.**

![image](https://user-images.githubusercontent.com/68090939/137104388-fa4038e5-e38a-418a-b541-7f5017458aa0.png)
<br>

NavController�� stateful�̾, �� ȭ���� �����ϴ� composable�� back stack�� �� ȭ���� ���¸� �����մϴ�. ������ ���� **rememberNavController() �Լ��� ���� NavController�� �����ϰ� �� ���¸� ����� �� �ֽ��ϴ�.**
<br>

`val navController = rememberNavController()`

<br>

**NavController�� ��� composable�� ������ �� �ִ� ��ġ�� �����ϴ� ���� �߿��մϴ�!** �̴� composable�� state�� caller������ �ѱ�� state hoisting ��Ģ�� ������ ���Դϴ�. ȭ�� �ܺο��� composable�� ������Ʈ�� ��, `currentBackStackEntryAsState()` �Լ��� �Ѱܹ��� NavController�� state�� ����Ϸ���, NavController�� ��� composable�� ���� ������ ��ġ�� ��������� �մϴ�.

<br>

**2. NavHost�� �����Ѵ�.**

![image](https://user-images.githubusercontent.com/68090939/137104684-896394c5-dfc0-42ae-826e-2e24f85bb9ef.png)<br>

�� NavController�� �ϳ��� NavHost composable�� ����Ǿ�� �մϴ�. **composable ���� �׺���̼��� �� ��, NavHost�� ������ �ڵ����� �籸���˴ϴ�.**<br>

ȭ���� �����ϴ� ���� composable���� **route�� ����** composable���� �̵� ��θ� �����ϰ�, �ٸ� composable�κ��� ���ڵ� ���� ���� �� �ֽ��ϴ�. �ٽ� ����, **�׺���̼� �׷����� �� destination�� route�� ����Ǿ� �ִ� ���Դϴ�.**

```kotlin
@Composable
public fun NavHost(
    navController: NavHostController,
    startDestination: String,
    modifier: Modifier,
    route: String?,
    builder: NavGraphBuilder.() �� Unit
): Unit
```
<br>

**NavHost�� �̸� ������ navController�� startDestination�� ���������ν� ������ �� �ֽ��ϴ�.** builder ����� ���� �׺���̼� �׷����� ����µ�, �̴� �Լ� �����̱� ������ �߰�ȣ�� ������ ������ ������ �ۼ��� �� �ֽ��ϴ�.

<br>

**3. navigate() �Լ��� ���ڿ� �̵��� ȭ���� route�� ���´�.**

![image](https://user-images.githubusercontent.com/68090939/137104941-5d2615cd-a216-44b2-ac5d-71e76a95f998.png)<br>


�׺���̼� �׷����� composable���� ���� �׺���̼��� �ϱ� ���ؼ�, navigate()��� �Լ��� ����� �� �ֽ��ϴ�. **navigate �Լ��� �Ű������� route�� ���� Ư�� composable�� ���ڸ� ������ �� �ֽ��ϴ�.**

<br>

**4. back stack �߰� ����**

![image](https://user-images.githubusercontent.com/68090939/137105101-fd925a7c-8332-4098-9fbf-1b4bf966d46d.png)<br>


�⺻������ navigate() �Լ��� **���ο� destination�� back stack�� �߰�**�մϴ�. �׸��� �߰����� �׺���̼� �ɼ��� navigate()�� ȣ�⿡ ���������ν� navigate�� ������ ������ �� �ֽ��ϴ�.<br>
���� ���, **popUpTo() �Լ�**�� ���ο� destination���� �׺���̼� �ϱ� ���� back stack���κ��� ���� destination�� ��� ������ �����ϴ�. (stack���� ������ push, ������ pop)<br>
**lauchSingleTop = true**�� ������ destination�� ���� ���纻�� ������ top�� ���̴� ���� �����մϴ�. ����, ������ ���� top�� �������� �ʾҴ� destination�� �׺���̼� �� �� �ְ� �մϴ�.<br>

<hr>
<br>

## CheggPrep �ۿ� Navigation Ȱ���ϱ�!

<br>

### 1. BottomNavigationBar�� ȭ�� ��ȯ�ϱ�

<br>

<div style="float:left; margin-right:20px;">
    <img src="https://user-images.githubusercontent.com/68090939/137105414-7e24f099-19b8-4a6d-87e9-14d4f5a9d0b6.png" width="300px"> 
</div>
<div style="float:left;">
    <img src="https://user-images.githubusercontent.com/68090939/137105437-831110d0-9c8e-4773-ac02-0bf85a1ebec8.png" width="300px">
</div>

<div style="float:left; margin-right:20px;">
    <img src="https://user-images.githubusercontent.com/68090939/137105457-ff326ef1-c086-4401-bdb4-a938a1f92524.png" width="300px"> 
</div>
<div style="float:left;">
    <img src="https://user-images.githubusercontent.com/68090939/137105476-b9dc1e5e-4a85-42a3-bfd4-7c83d76b1a2a.png" width="300px">
</div><div style="clear:both"></div>


��ó�� BottomNavigationBar���� Ŭ���� �����ܿ� ���� �ٸ� ȭ���� ���� �� �ְ� �����ô�!<br><br>

<img src="https://user-images.githubusercontent.com/68090939/137106417-a214eca9-dd36-4637-a145-d29622146c63.png" width="350px">

�ϴ�, �� ������ ���� navigation ��Ű���� navigation.kt ������ �����ؼ� ������ ���� �ڵ带 �ۼ����ݴϴ�.

```kotlin
package com.gdsc.cheggprepreview.navigation

import ...

sealed class Screen(val route: String){
    object Home: Screen("home")
    object Search: Screen("search")
    object Create: Screen("create")
    object More: Screen("more")
    object Deck: Screen("deck")
}

data class BottomNavItem(
    val route: String,
    val name: String,
    val icon: ImageVector
)

object BottomNav{
    val items = listOf(
        BottomNavItem(Screen.Home.route, "Home", Icons.Outlined.Home),
        BottomNavItem(Screen.Search.route, "Search", Icons.Outlined.Search),
        BottomNavItem(Screen.Create.route, "Create", Icons.Outlined.AddBox),
        BottomNavItem(Screen.More.route, "More", Icons.Outlined.Menu),
    )
}

@Composable
fun BottomNavigationBar(navController: NavController) {
    val navBackStackEntry by navController.currentBackStackEntryAsState()
    val currentRoute = navBackStackEntry?.destination?.route

    BottomNavigation(
        backgroundColor = Color.White,
        elevation = 4.dp,
        modifier = Modifier.padding(4.dp)
    ){
        BottomNav.items.forEach { item ->
            BottomNavigationItem(
                selected = item.route == currentRoute,
                enabled = item.route != currentRoute,
                onClick = { navController.navigate(item.route) },
                icon = { Icon(imageVector = item.icon, contentDescription = item.name) },
                label = { Text(item.name) },
                selectedContentColor = DeepOrange,
                unselectedContentColor = Color.DarkGray
            )
        }
    }
}
```
<br>

- sealed class�� Screen�� �����ϰ�, object�� Home, Search, Create, More��� 4���� ȭ���� �����մϴ�. (Deck ȭ���� �ڿ��� �����մϴ�.)
- BottomNavigation() composable�� ��Ƽ���� �������� API�̹Ƿ� Alt + Enter ����Ű�� import�� �� ���ݴϴ�. ��ȯ�� ȭ����� items ����Ʈ�� BottomNavItem�� �����մϴ�.

<br>

- ���� MainActivity���� BottomNavigationBar�� ��ġ ���θ� ���ϰ�, NavHost�� block �κп��� ��ȯ�� composable���� �����մϴ�.

```kotlin
	val navController = rememberNavController()
	
	val (bottomBarShown, showBottomBar) = remember {
	    mutableStateOf(true)
	}
	
	Scaffold(
	    bottomBar = {
	        if (bottomBarShown) {
	            BottomNavigationBar(navController = navController)
	        }
	    }
	) {
	    NavHost(navController = navController, startDestination = Screen.Home.route) {
	        composable(Screen.Home.route) {
	            showBottomBar(true)
	            HomeScreen(navController)
	        }
	
	        composable(Screen.Search.route) {
	            showBottomBar(true)
	            SearchScreen(navController)
	        }
	
	        composable(Screen.Create.route) {
	            showBottomBar(false)
	            CreateScreen(navController)
	        }
	
	        composable(Screen.More.route) {
	            showBottomBar(true)
	            MoreScreen(navController)
	        }
			}
	}
```
<hr>
<br>

### 2. DeckScreen ����

<br>

<img src="https://user-images.githubusercontent.com/68090939/137108405-1985e6ee-ab72-4870-babf-f6eb60782b2a.png" width="400px">


DeckScreen�� �� DeckItem�� Ŭ������ �� ��Ÿ���� ȭ���Դϴ�. 

<br>

<img src="https://user-images.githubusercontent.com/68090939/137108636-e4e29d57-6c76-46ca-ae89-d8505a2bc750.png" width="350px">


����, screens ��Ű���� DeckScreen.kt �̶�� ������ �����ϰ� ������ ���� �ڵ带 �ۼ����ݴϴ�.<br>

```kotlin
package com.gdsc.cheggprepreview.screens

import ...

@Composable
fun DeckScreen(navController: NavController, title: String, cardsNum: Int) {
    Scaffold(topBar = {
        TopAppBar(
            elevation = 0.dp,
            backgroundColor = Color.White,
            title = { Text(text = title) },
            navigationIcon = {
                IconButton(onClick = { navController.popBackStack() }) {
                    Icon(
                        imageVector = Icons.Default.ArrowBack,
                        contentDescription = "navigation back"
                    )
                }
            },
            actions = {
                IconButton(onClick = { /* �����ϱ� ��ư Ŭ������ �� */ }) {
                    Icon(imageVector = Icons.Default.Share, contentDescription = "share")
                }
                IconButton(onClick = { /* ������ ��ư Ŭ������ �� */}) {
                    Icon(imageVector = Icons.Default.MoreVert, contentDescription = "more")
                }
            }
        )
    }, bottomBar = {
        Column(modifier = Modifier.background(color = Color.White)) {
            Divider(Modifier.height(2.dp), color = Color.LightGray)
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(vertical = 16.dp),
                horizontalArrangement = Arrangement.Center,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Row(modifier = Modifier
                    .clip(shape = CircleShape)
                    .clickable { /* ī�� �����ϱ� �ؽ�Ʈ Ŭ������ �� */ }
                    .background(color = DeepOrange)
                    .padding(horizontal = 24.dp, vertical = 8.dp)
                ) {
                    Text(
                        text = "Practice all cards",
                        color = Color.White,
                        style = MaterialTheme.typography.h5,
                        fontWeight = FontWeight.ExtraBold
                    )
                }
            }
        }
    }) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(
                text = cardsNum.toString() + if (cardsNum > 1) " Cards" else " Card",
                color = Color.Gray
            )
            Spacer(modifier = Modifier.height(16.dp))
            repeat(cardsNum) {
                CardItem(card = Card("Title", "description"))
                Spacer(modifier = Modifier.height(8.dp))
            }
        }
    }
}
```

<br>


    1. DeckScreen������ BottomNavigationBar�� �������� ���� ���̱� ������ MainActivitiy���� showBottomBar()�� false�� �������ݴϴ�.

```kotlin
composable(Screen.Deck.route + "/{deckTitle}/{cardsNum}") { backStackEntry ->
    val deckTitle =
        backStackEntry.arguments?.getString("deckTitle") ?: "no title"
    val cardsNum =
        backStackEntry.arguments?.getString("cardsNum")?.toInt() ?: 0
    showBottomBar(false)
    DeckScreen(
        navController = navController,
        title = deckTitle,
        cardsNum = cardsNum
    )
}
```

<br>

    2. Scaffold�� Ȱ���� topBar�� �����غ��ô�. TopAppBar composable�� ũ�� navigationIcon, title, actions �� �������� �������ݴϴ�. �ڷΰ��� ȭ��ǥ�� ������ navController.popBackStack() �� �޼ҵ带 ���� ���� ȭ������ ���ư��� �ϰ�, �����ϱ�� ������ ������ ��ư�� �߰����ݴϴ�. (�ڷΰ��� navigationIcon�� CreateScreen�� Close �����ܿ��� �Ȱ��� �������ݴϴ�.) <br> title�� ���� deck�� �ش��ϴ� Ÿ��Ʋ�� ������� �ϱ� ������ back stack���� ���ڸ� �����ͼ� �����ݴϴ�.


```kotlin
Scaffold(topBar = {
        TopAppBar(
            elevation = 0.dp,
            backgroundColor = Color.White,
            title = { Text(text = title) },
            navigationIcon = {
                IconButton(onClick = { navController.popBackStack() }) {
                    Icon(
                        imageVector = Icons.Default.ArrowBack,
                        contentDescription = "navigation back"
                    )
                }
            },
            actions = {
                IconButton(onClick = { /* �����ϱ� ��ư Ŭ������ �� */ }) {
                    Icon(imageVector = Icons.Default.Share, contentDescription = "share")
                }
                IconButton(onClick = { /* ������ ��ư Ŭ������ �� */}) {
                    Icon(imageVector = Icons.Default.MoreVert, contentDescription = "more")
                }
            }
        )
    }, bottomBar = { ...
```

<br>

    3. ���� bottomBar �κ��� ���캾�ô�! �ϴ�, "Practice all cards" ��� �ؽ�Ʈ�� ���� �Ʒ��� �����ְ�, ���� Column���� repeat�� ����ؼ� ���ڷ� ���� ���� cardsNum ������ŭ CardItem�� �����ݴϴ�!

```kotlin
... }, bottomBar = {
        Column(modifier = Modifier.background(color = Color.White)) {
            Divider(Modifier.height(2.dp), color = Color.LightGray)
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(vertical = 16.dp),
                horizontalArrangement = Arrangement.Center,
                verticalAlignment = Alignment.CenterVertically
            ) {
                Row(modifier = Modifier
                    .clip(shape = CircleShape)
                    .clickable { /* ī�� �����ϱ� �ؽ�Ʈ Ŭ������ �� */ }
                    .background(color = DeepOrange)
                    .padding(horizontal = 24.dp, vertical = 8.dp)
                ) {
                    Text(
                        text = "Practice all cards",
                        color = Color.White,
                        style = MaterialTheme.typography.h5,
                        fontWeight = FontWeight.ExtraBold
                    )
                }
            }
        }
    }) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(
                text = cardsNum.toString() + if (cardsNum > 1) " Cards" else " Card",
                color = Color.Gray
            )
            Spacer(modifier = Modifier.height(16.dp))
            repeat(cardsNum) {
                CardItem(card = Card("Title", "description"))
                Spacer(modifier = Modifier.height(8.dp))
            }
        }
    }
}
```

<br>

    4. CardItem�� ������ �ۼ��ߴ� text �κ��� card.front, card.back���� ��������� �մϴ�!

```kotlin
package com.gdsc.cheggprepreview.models

// �ո�� �޸��� �ؽ�Ʈ�� ī�带 �����Ѵ�.
data class Card(val front: String, val back: String)
```

```kotlin
@Composable
fun CardItem(card: Card) {
    Column(
        modifier = Modifier
            .fillMaxWidth()
            .border(width = 2.dp, color = Color.LightGray)
    ) {
        Text(
            text = card.front,
            modifier = Modifier.padding(16.dp),
            fontWeight = FontWeight.ExtraBold
        )
        // ���м� �����
        Divider(
            modifier = Modifier
                .fillMaxWidth()
                .height(2.dp),
            color = Color.LightGray
        )
        Text(
            text = card.back,
            modifier = Modifier.padding(16.dp),
            color = Color.Gray
        )
    }
}
```

<br>
<hr>
<br>

<div style="float:left; margin-right:20px;">
    <img src="https://user-images.githubusercontent.com/68090939/137109445-e2be7fce-e98f-4b70-85c7-e4017b806225.png" width="300px"> 
</div>
<div style="float:left;">
    <img src="https://user-images.githubusercontent.com/68090939/137109476-2f2f6a01-7455-4d1a-8f64-23c88764ff0c.png" width="300px">
</div><div style="clear:both"></div>
<br>

title�� cardsNum �� �ΰ����� ���� ����ó�� deck ������ ���� �޶����� �ϱ� ������, ������ �� �����մϴ�.

<br>

    1. HomeScreen���� DeckItem�� Ŭ���� ��, Screen.Deck.route�� deckTitle, cardsNum ���ڸ� �߰��Ѵ�.

```kotlin
@Composable
public fun DeckItem(
    deck: Deck,
    modifier: Modifier,
    onClick: () �� Unit
): Unit
```

```kotlin
LazyColumn(modifier = Modifier.padding(16.dp)) {
    when(selectedFilterIndex){
        0-> // SampleDataSet�� ��� ����Ʈ �����ֱ�
            SampleDataSet.deckSample.forEach{
                item{
                    DeckItem(deck = it, modifier = Modifier.padding(bottom = 8.dp))
                    {
                        navController.navigate(
                            Screen.Deck.route + "/${it.deckTitle}/${it.cardList.size}"
                        )
                    }
                }
            }
				... (����)
}
```

<br>

    2. MainActivity���� ������ ���� composable�� backStackEntry�� �ִ� deckTitle, cardsNum�� ���� ��, DeckScreen composable�� �����Ѵ�.

```kotlin
composable(Screen.Deck.route + "/{deckTitle}/{cardsNum}") { backStackEntry ->
    val deckTitle =
        backStackEntry.arguments?.getString("deckTitle") ?: "no title"
    val cardsNum =
        backStackEntry.arguments?.getString("cardsNum")?.toInt() ?: 0
    showBottomBar(false)
    DeckScreen(
        navController = navController,
        title = deckTitle,
        cardsNum = cardsNum
    )
}
```

<br>

    3. DeckScreen�� ���ڷ� ���� deckTitle, cardsNum�� ����Ѵ�.

```kotlin
@Composable
fun DeckScreen(navController: NavController, title: String, cardsNum: Int)
```

<br>

���� �ð��� �ڵ带 ����ġ�⸸ �� ���� �̷��� ������ ������ �� �����µ�, Ctrl + B�� ����ؼ� �ٿ����� ã�ư����� �̷��Գ� ���� ���ϵ��� �����ִ� ����� ������ �����̾����ϴ�. <br>

���⼭ ����� ����, 

- **destination - navigation�� ������, �̵��� ��� ȭ��**
- **route�� ���� navigate (�ٸ� ������ ȭ������ �̵�)**
- **BackStackEntry - navigation�� ���� ����ϴ� ����**

�ٷ� �� �����Դϴ�. ó������ �̰� ���� �Ҹ����� �� �ʹ��� �ʾҴµ�, CheggPrep���� ���� ����� �غ��� ���� ���� ���� ������ �����Դϴ�!

���������� �ٽ� �������ڸ�,
- destination�� argument�� �����ϴ� ����� route�� �ش� ���ڸ� �߰��ϴ� ���Դϴ�.
- �� ���ڴ� backstackEntry�� arguments�� Bundle ���·� ����Ǳ� ������, �̸� �ٽ� �������� getString ���� �Լ��� ���� Ű�� �ش��ϴ� ���� ������ �˴ϴ�.

<br>

cf) ���������� �򰥷ȴ� �κ�

- Screen.Deck.route�� Deck�� sealed class Screen���� ������ ȭ�� �� �ϳ��̰�,

```kotlin
sealed class Screen(val route: String){
    object Home: Screen("home")
    object Search: Screen("search")
    object Create: Screen("create")
    object More: Screen("more")
    object Deck: Screen("deck")
}
```
<br>

- DeckItem�� Deck�� models ��Ű���� �ִ� ������ Ŭ�����̴�.

```kotlin
package com.gdsc.cheggprepreview.models

data class Deck(
    val deckType: Int,
    val deckTitle: String,
    val shared: Boolean,
    val bookmarked: Boolean,
    val cardList: List<Card> // deck�� ���Ե� Card�� ����Ʈ
)

// deckType ����
const val DECK_CREATED = 0
const val DECK_ADDED = 1
```

<br>
<hr>
<br>

# ������

�Ϳ� ���� �����׿�! &#128079;&#128079;&#128079; ���� �ð����� �׺���̼��̶�� ���䵵 ��������, �ڵ嵵 �ƹ� �������� ����ġ�⸸ �ߴ� �� ������, �̷��� ������ �ϰ� ���� ���� �ۼ��� �ڵ尡 � ������ �����ϴ��� ���� �� ���ذ� �˴ϴ�!<br>
Ȥ�� ���� ������ ���� �߿� �߸��� �κ��� �ִٸ� �� ��� �����ֽñ� �ٶ��ϴ�! ����Ⱓ ������ �ٽ� �����غ��Կ�! �߰���簡 2�ֵ� �� ���Ҵٴ� �� �����������, �׷��� 4���� ���� ���� �ؾ������ ���� �����صּ� �����Դϴ�! �ٵ� ����Ⱓ �������Ͻð� 2�� �ڿ� ������~ &#128075;&#128075;



