---
title: "How to do unit test for golang program?"
date: 2022-05-14T18:18:55+08:00
draft: false
---


>> 實作一個功能能夠計算出水仙花數，並且提供一個正確的測試案例，一個錯誤的測試案例，測試完執行結果正確。   
>>> Last edited : Cheng-Kai Cheng  
>> Language : Golang


### 1. 甚麼是水仙花數(Narcissistic number又被稱作 Armstrong Number)?

先上 wiki找，了解一下[水仙花數](https://zh.wikipedia.org/zh-tw/%E6%B0%B4%E4%BB%99%E8%8A%B1%E6%95%B0)

用一個簡單的例子來說明:

`370 = 3^3 + 7^3 + 0^3。`

370 是三位數，所以次方為 3，然後每個數字的 3次方的和為 370。


既然理解需求了，我們就實際來處理需求吧!

1. 打開 Goland IDE
2. 新增一個 package， narcissisticNumberCal
3. 底下新增一個 go file, narcissisticNumberCal.go

#### 代碼如下

```golang
   package narcissisticNumberCal

   import "math"


   func CalculateIsArmstrong(n int) bool{
	a := n / 100
	b := n % 100 / 10
	c := n % 10
	return n == int(math.Pow(float64(a), 3) + math.Pow(float64(b), 3) + math.Pow(float64(c),3))

   }
```

### 2. 我們要如何做單元測試，證明這個 func 是正確的呢??

   //在 同樣目錄底下新增一個 go file, narcissisticNumberCal_test.go

   //代碼如下

```golang
   package narcissisticNumberCal_test

   import (
	"awesomeProject1/narcissisticNumberCal"
	"testing"
   )
   // 抽象化宣告
   type TestCase struct {
	value int
	expected bool
	actual bool
   }

   // 因為 371 是水仙花數，我們實體化 testCase，
   // 並且賦值給 testCase，期望為正確的(true)
   func TestCalculateIsArmstrong(t *testing.T) {
	testCase := TestCase{
		value: 371,
		expected: true,
	}
    // 調用你寫的 func，並帶入 371，看跟 testCase 是否相同?
	testCase.actual = narcissisticNumberCal.CalculateIsArmstrong(testCase.value)
    // 若相同則為不抱錯
	if testCase.actual != testCase.expected {
		t.Fail()
	}
   }
```
   //執行結果

```golang
  PS C:\Users\Kevin Cheng\GolandProjects\awesomeProject1> go test ./... -v
  === RUN   TestCalculateIsArmstrong
  --- PASS: TestCalculateIsArmstrong (0.00s)
  PASS
  ok      awesomeProject1/narcissisticNumberCal   0.234s
```



   //到了這邊，我們會思考一件事情，就是那錯誤案例怎麼測?同樣道理
   //新增一個測試 negative 的 func
   //代碼如下:

```golang
   package narcissisticNumberCal_test

   import (
	"awesomeProject1/narcissisticNumberCal"
	"testing"
   )

   type TestCase struct {
	value int
	expected bool
	actual bool
   }

   func TestCalculateIsArmstrong(t *testing.T) {
	testCase := TestCase{
		value: 371,
		expected: true,
	}

	testCase.actual = narcissisticNumberCal.CalculateIsArmstrong(testCase.value)

	if testCase.actual != testCase.expected {
		t.Fail()
	}
   }


   //因為 350 不是水仙花數，所以我們期望是錯誤的，那麼就應該在調用我們 func 回傳 false
   func TestNegativeCalculateIsArmstrong(t *testing.T){
	testCase := TestCase{
		value: 350,
		expected: false,
	}

	testCase.actual = narcissisticNumberCal.CalculateIsArmstrong(testCase.value)

	if testCase.actual != testCase.expected {
		t.Fail()
	}
   }
```

執行結果

```golang
   PS C:\Users\Kevin Cheng\GolandProjects\awesomeProject1> go test ./... -v
   === RUN   TestCalculateIsArmstrong
   --- PASS: TestCalculateIsArmstrong (0.00s)
   === RUN   TestNegativeCalculateIsArmstrong
   --- PASS: TestNegativeCalculateIsArmstrong (0.00s)
   PASS
   ok      awesomeProject1/calculator      0.193s
```
