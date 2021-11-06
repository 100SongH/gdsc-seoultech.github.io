---
layout: post
title: Android 5���� ���� ����
date: 2021-11-06
description: 
author: Hanyoonjae
categories: ["android"]
---

�ȳ��ϼ���! GDSC Android ��� �������Դϴ�.<br>
5���� ���� ���� ������ �����ϰڽ��ϴ�!
<br>

# 1. Navigation destination���� ȭ�� ��ȯ

## 1) SearchScreen

![SearchScreen](https://user-images.githubusercontent.com/71068767/140611561-146edde7-3034-4c96-9278-c0955d80ebae.png)

�� �׸��� ���� Search������ 3���� ȭ����ȯ�� �ʿ��մϴ�.<br>

������ ȭ���� �þ navigation destination�� ������ �߰��ϱ�� ����������ϴ�. <br>

���� �� ȭ���� ����, ���� composable�� ������ SearchScreen �ȿ��� ȣ�����ݴϴ�. <br>

�̸� ���ؼ��� SearchScreen �ȿ� �����͸� �����ؾ� �մϴ�. �� ȭ�麰 composable�� ������ ���� setter�� �����Ͽ� ������ �� �ֵ��� �ϴ� ������. State hoisting��� �� �� �ֽ��ϴ�. <br>

```kotlin
enum class SearchState { //Searchscreen�� ���� composable ����
    ButtonScreen,
    QueryScreen,
    ResultScreen
}
```

���� enum class�� ȭ�� ���¸� ��Ÿ���� SearchState�� �����մϴ�.<br>

enum class�� ������ Ŭ������ �� ����� ��ü�Դϴ�. ���� �� ��� �̸����� ȭ�� ���¸� �����մϴ�. (�ڼ��� [enum class](https://kotlinlang.org/docs/enum-classes.html#working-with-enum-constants)�� ������ ���� ����Ʈ�� ��ũ�� Ÿ�� ���ּ���!)

```kotlin
val (screenState, setScreenState) = remember { // setScreenState�� ȭ�� ��ȯ
        mutableStateOf(SearchState.ButtonScreen)
    }

val (queryString, setQueryString) = remember { // �˻� �ܾ�� QueryScreen���� ����, ResultScreen���� ���
        mutableStateOf("")
    }
```

���� ���� SearchScreen �ȿ� ���¸� �����մϴ�. 

```kotlin
when (screenState) { // ������ ȭ�鿡�� �۵��� ���� �����ش�.
        SearchState.ButtonScreen -> {
            SearchButtonScreen {
                if(queryString.isNotBlank()){ //�� �� �˻� �� �ٽ� QueryScreen�� ������ ���ִ� ���ǹ�
                    setScreenState(SearchState.ResultScreen)
                }else{
                    setScreenState(SearchState.QueryScreen)
                }
                setScreenState(SearchState.QueryScreen)
            }
        }
        SearchState.QueryScreen -> {
            SearchQueryScreen(
                queryString = queryString,
                setQueryString = setQueryString,
                toButtonScreen = { setScreenState(SearchState.ButtonScreen) },
                toResultScreen = { setScreenState(SearchState.ResultScreen) }
            )
        }
        SearchState.ResultScreen -> {
            SearchResultScreen(
                queryString = queryString,
                setQueryString = setQueryString,
                toButtonScreen = { setScreenState(SearchState.ButtonScreen) },
                onSearchKey = { /*�˻� ��� ������Ʈ*/ }
            )
        }
    }
```

when ���� screenState�� ���� �ٸ� composable �ε��մϴ�. <br>

������ ������ screenState�� queryString�� �������� Ȱ���ϰ� ������ ȭ���� ��ȯ�� �� �ֽ��ϴ�.

```kotlin
@Composable
fun SearchResultScreen( //�Ķ���ͷ� ������ ���� �޾� �۵�!
    queryString: String,
    setQueryString: (String) -> Unit,
    toButtonScreen: () -> Unit,
    onSearchKey: () -> Unit
) {
    Scaffold(
        topBar = {
            SearchTopBar(
                queryString = queryString,
                setQueryString = setQueryString,
                onBackButtonClick = toButtonScreen,
                onSearchKey = onSearchKey/*Todo �˻� ��� ������Ʈ*/
            )
        }
                        "...(����)..."
```

�Ķ���� �ۼ����� navigation�� �۵��մϴ�. SearchQueryScreen�� ���� �����ϴ�.
    
```kotlin
@Composable
fun SearchTopBar(
    queryString: String,
    setQueryString: (String) -> Unit,
    onBackButtonClick: () -> Unit,
    onSearchKey: () -> Unit
) {
    TopAppBar(
        elevation = 0.dp,
        backgroundColor = Color.White,
        navigationIcon = {
            IconButton(onClick = onBackButtonClick) {
                        "...(�߷�)..."

                keyboardOptions = KeyboardOptions( //������ ��ư
                    imeAction = ImeAction.Search
                ),
                keyboardActions = KeyboardActions(
                    onSearch = {
                        onSearchKey()
                    }
                ),
            )
        },
        actions = {
            if (queryString.isNotBlank()) {
                IconButton(onClick = { setQueryString("") }) {
                    Icon(imageVector = Icons.Default.Close, contentDescription = "delete")
                }
            }
        }
    )
}
```

SearchScreen ���� topbar�� ��� ���־ ���� composable�� ������ݴϴ�. <br>

�� bar������ �˻��� �� �� �ֵ��� �ϴµ� keyboardOptions�� keyboardActions�� �ڵ������� �ؽ�Ʈ�� �Է��� �� ��Ÿ���� Ű������ �˻� ��ư�� �����մϴ�. ��ư�� ������ �˻� ����� �����ִ� onSearchKey()�� �־ ������. ([KeyboardOptions](https://developer.android.com/reference/kotlin/androidx/compose/foundation/text/KeyboardOptions)�� ���� �ڼ��� ������ Ŭ��!)

�� ��ư ���� ���� ���� �ð��� �ٷ����Ƿ� ��ŵ!

[�����ڷ�](https://blog.naver.com/comye1/222555753843)

## 2) CreateScreen
![CreateScreen](https://user-images.githubusercontent.com/71068767/140611545-2ae99d5d-4bbd-4429-b393-a5d085d4b17b.png)

������ CraeteScreen�Դϴ�. <br>


�ռ� SearchScreen���� �ٷ� navagation ����� ���� ����մϴ�. ȭ�� ���´� 2�����Դϴ�.

```kotlin
enum class CreateState { //CreateScreen�� ���� composable ����
    TitleScreen,
    CardScreen
}
```

enum class�� ���¸� �����ϰ�,

```kotlin
val (deckTitle, setDeckTitle) = remember {
        mutableStateOf("")
    }

    val (visibility, setVisibility) = remember {
        mutableStateOf(true)
    }

    val (screenState, setScreenState) = remember {
        mutableStateOf(CreateState.TitleScreen)
    }
```

���¸� screenState�� �����մϴ�.

```kotlin
when (screenState) {
        CreateState.TitleScreen -> {
            CreateTitleScreen(
                deckTitle = deckTitle,
                setDeckTitle = setDeckTitle,
                visibility = visibility,
                setVisibility = setVisibility,
                navigateBack = { navController.popBackStack() },
                toCardScreen = { setScreenState(CreateState.CardScreen) }
            )
        }
        CreateState.CardScreen -> {
            CreateCardScreen(
                navigateBack = { navController.popBackStack() },
                onDone = {}
            )
        }
    }
```

when���� ȭ���� ���� TitleScreen������  decktitle, visibility, navigateBack, ScreenState�� ���� �ް�, CardScreen���� �Ѿ���� navigateBack�� onDone�� ������ �� �ֵ��� �մϴ�.

```kotlin
fun CreateCardScreen(
    navigateBack: () -> Unit,
    onDone: () -> Unit
) {
    Scaffold(
        topBar = {
            TopAppBar(
                        "...(�߷�)..."

                navigationIcon = {
                    IconButton(onClick = navigateBack) { // close navi
                        Icon(
                            imageVector = Icons.Outlined.Close,
                            contentDescription = "close create screen"
                        )

                    }
                },
                actions = {
                    TextButton(onClick = onDone) {
                        Text(
                            "Done", style = MaterialTheme.typography.h6,
                            fontWeight = FontWeight.Bold
                        )
                    }
                }
            )
        },
        floatingActionButton = { //��ũ���� ������ ���� �������� ��ư
            FloatingActionButton(
                onClick = { /*TODO*/ },
                backgroundColor = Color.White,
                modifier = Modifier
                    .size(48.dp)
                    .border(
                        width = 2.dp,
                        color = DeepOrange,
                        shape = CircleShape
                    )
            ) {
```

���� �ڵ带 ���� composable CreateCardScreen �ȿ� �Ķ���ͷ� navigateBack�� onDone�� �����Ǿ� �ֽ��ϴ�.<br>

�� ���� ���� onClick�� �ش� �Ķ������ �̸��� �ٿ��� ���� Ȯ���� �� �ֽ��ϴ�. ������ navController�� �۵��ϴ� ���� ������ ���� �̸����� �����͸� composable���� �ְ� ���� �� �ְ� �մϴ�. �� ����ϰ� ������ ���Դϴ�.
                
# 2. View Model

���� �ֿ� ������ **View Model**�� ���� �˾ƺ��ô�.<br>
<br>
Android���� Activity�� **configuration change**�� �߻��� ������ onCreate�� ȣ���ؼ� View�� �ٽ� �ε��մϴ�. Activity�� ������ ���� ���ε�Ǵ� ������. �̶� �䰡 ������ �ִ� �����ʹ� ��� ���ư��ϴ�.<br>
<br>
**configuration change**�� ȭ�� ȸ��, ��Ƽ ȭ�� �� ȭ���� �޶����� ��ȭ�� ���մϴ�.<br>
<br>

![View1](https://user-images.githubusercontent.com/71068767/140611566-a7163daa-44e0-4f1b-a6c9-60c7d990d03f.png)

�̷� ��Ȳ�� �����Ϸ��� �����͸� �Ͻ������� �����ؾ��մϴ�. �׷��� **View Model**�� �����մϴ�. View Model�� Activity�� ������ ���� ������(finished) �����˴ϴ�.<br>
<br>

�ڼ��� Activity �����ֱ�� ���� ��ũ���� �˾ƺ� �� �ֽ��ϴ�.<br>
[https://blog.naver.com/comye1/222280060131](https://blog.naver.com/comye1/222280060131)<br>
<br>

![View2](https://user-images.githubusercontent.com/71068767/140611568-d0576852-665b-4f32-a3bd-dbbe3c57393a.png)

�׸��� ���� composable�� View Model�� ������ ������ �ֽ��ϴ�<br>
<br>
�����Ͱ� ������ view model�� �����Ǿ� �ְ�, �� �����͸� activity ���� composable�� �޾� ȭ���� �����ݴϴ�. ����ڿ��� ��ȣ�ۿ�, ��� ���õ� ������ ����ϰ� �ֽ��ϴ�.<br>
<br>
�̷��� ���ϸ� �����ϱ� ������, ���� �׸��� �ѹ� ���ô�.<br>
<br>

![View3](https://user-images.githubusercontent.com/71068767/140611571-5f690fb8-6b9a-4619-b5f3-4079d6f69042.png)

�� �׸����� �� �� �ֵ��� ViewModel�� Activity�� ���� state�� event�� �ְ� �޽��ϴ�.<br>
<br>
View Model���� UI�� ���� state�� ������Ʈ�Ǹ�, flow down���� Activity�� ���޵˴ϴ�. ���޹��� state�� event�� �߻��ϰ�, �̴� �ٽ� View Model�� flow up�˴ϴ�.<br>
<br>
�� ������ �ݺ��Ǹ鼭 android ȭ���� ��ȭ�մϴ�.<br>
<br>

![View4](https://user-images.githubusercontent.com/71068767/140611572-b9ffd892-f619-4404-a0bb-0efc77ae2d02.png)

state hoisting�� �Ͼ ���� 3���� ������ �ֽ��ϴ�.<br>
<br>
1. State�� ��� state�� ����ϴ� ��� ���������� **���� ���� ���� �θ�**(���� ����� ���� composable)�� ȣ�̽��õǾ�� �Ѵ�.
2. State�� ���� �Ǵ� ������ �� �ִ� ��� �ְ��� �������� ȣ�̽��õǾ�� �Ѵ�.
3. ���� �� ���� state�� ���� �̺�Ʈ�� ���� �����ϸ� �׵��� �Բ� ȣ�̽��õȴ�.<br>
-> �ش� ��Ģ���� �� ���� state�� ȣ�̽�Ʈ�� ���� ������, ������ ȣ�̽����� �ϴ� ���� �Ұ����ϴ�.

[State Hoisting](https://developer.android.com/jetpack/compose/state#state-hoisting)�� ������ �˸� View Model�� ������ �� �� ������ �� �ֽ��ϴ�!<br>
<br>

![View5](https://user-images.githubusercontent.com/71068767/140611573-87a311ed-e64d-4e29-973f-f1bdb0ca01ae.png)

Composable�� State Ÿ���� observe�ϱ� ������ viewmodel���� state�� ����Ǵ� ���� �����ϴ�. �ٷ� ������ �����ϱ� ��������.<br>
<br>
������ LiveData�� ����Ǿ��� ���� �����ϴ� composable�� observeAsState�� overhead�Ǵ� ���� �� �� �ֽ��ϴ�.<br>
<br>
Livedata�� mutableStateListOf�� ��ü�� �����մϴ�.<br>
<br>

Livedata�� ���� ��ũ���� �˾ƺ� �� �ֽ��ϴ�.
[https://developer.android.com/topic/libraries/architecture/livedata?hl=ko](https://developer.android.com/topic/libraries/architecture/livedata?hl=ko)

# ������

�� 5���� ������ �������ϴ�. ���� ȸ���� �þ ���� �ǽ��� ���Ǵ� ������ ��ư� �����ϳ׿�.. ViewModel�� ������ ������� ���� ���鼭�� ���Ĺ����� ���� �����̵��� ������ �״�� �������� ǥ���� ���� ���� �����ϴ�. �ٸ� �е�ó�� ���ذ� �� �Ǵ� �������� �ۼ��ϰ� �;��µ�, �ƽ��� ������ Ů�ϴ�. :disappointed_relieved:<br>
<br>

������ �̰͵� ��������������! ������ ������ �׿� �Ƿ��� �Ǵ� �Ŵϱ�� ���� �ȵ���̵� ȭ����!! :tada: :tada: