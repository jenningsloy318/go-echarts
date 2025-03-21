<p align="center">
<img src="https://user-images.githubusercontent.com/19553554/52535979-c0d0e680-2d8f-11e9-85c8-2e9f659e7c6f.png" width=300 height=300 />
</p>

<h1 align="center">go-echarts</h1>
<p align="center">
    <em>🎨 The adorable charts library for Golang.</em>
</p>

<p align="center">
    <a href="https://github.com/go-echarts/go-echarts/actions/workflows/ci.yml">
        <img src="https://github.com/go-echarts/go-echarts/actions/workflows/ci.yml/badge.svg" alt="Build Status">
    </a>
    <a href="https://goreportcard.com/report/github.com/go-echarts/go-echarts">
        <img src="https://goreportcard.com/badge/github.com/go-echarts/go-echarts" alt="Go Report Card">
    </a>
	<a href="https://github.com/go-echarts/go-echarts/pulls">
        <img src="https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat" alt="Contributions welcome">
    </a>
    <a href="https://opensource.org/licenses/MIT">
        <img src="https://img.shields.io/badge/License-MIT-brightgreen.svg" alt="MIT License">
    </a>
        <a href="https://pkg.go.dev/github.com/go-echarts/go-echarts/v2">
        <img src="https://godoc.org/github.com/go-echarts/go-echarts?status.svg" alt="GoDoc">
    </a>
    <a href="https://echarts.apache.org/">
        <img src="https://img.shields.io/badge/echarts-_v5.4.3-orange" alt="echartsVersion">
    </a>
</p>

> 如果一门语言可以用来写爬虫，那么它就需要一个优雅的数据可视化库。 --- by dongdong

在 Golang 这门语言中，目前数据可视化的第三方库还是特别少，[go-echarts](https://github.com/go-echarts/go-echarts)
的开发就是为了填补这部分的空隙。  
[Apache ECharts](http://echarts.apache.org/) 是非常优秀的可视化图表库，凭借着良好的交互性，精巧的图表设计，得到了众多开发者的认可。
也有其他语言为其实现了相应语言版本的接口，如 Python 的 [pyecharts](https://github.com/pyecharts/pyecharts)，
go-echarts 也从 pyecharts 借鉴了一些设计思想。

### 🔰 安装

GOPATH

```shell
$ go get -u github.com/go-echarts/go-echarts/...

# 因为 gomod 的特殊的版本管理方式，使用 go get 方式并不能直接使用 v2 go-echarts 🐶
# 不过可以通过以下方法使用新版本...
$ cd $go-echarts-project
$ mkdir v2 && mv charts components datasets opts render templates types v2
```

GoMod

```shell
require github.com/go-echarts/go-echarts/v2
```

### ⏳ 版本

go-echarts 现在有 v1 和 v2 两个大版本，v1 版本的代码位于 v1 分支下，v2 代码位于 master 分支下。两个大版本是不兼容的，且 v1
不会再维护了，所以欢迎使用 v2 版本。

特别的，当有一些小的改动时（通常做的部分重构）会先发布rc版本，表示这是一个待发布的新的可能有小改动的版本（非breaking）。
如果跨rc版本对已有的项目直接升级时，可能需要小调整一下代码。

### ✨ 特性

* 简洁的 API 设计，使用如丝滑般流畅
* 囊括了 **25+** 种常见图表，应有尽有
* 高度灵活的配置项，可轻松搭配出精美的图表
* 详细的文档和示例，帮助开发者更快的上手项目
* 多达 **400+** 地图，为地理数据可视化提供强有力的支持

### 📝 使用

使用`go-echarts`
可以参考[handbook](https://go-echarts.github.io/go-echarts), [go-echarts/examples](https://github.com/go-echarts/examples)
和 [GoDocs](https://pkg.go.dev/github.com/go-echarts/go-echarts/v2).  
仅需要几行核心代码就可画出美观的图表

```golang
package main

import (
	"math/rand"
	"os"

	"github.com/go-echarts/go-echarts/v2/charts"
	"github.com/go-echarts/go-echarts/v2/opts"
)

// generate random data for bar chart
func generateBarItems() []opts.BarData {
	items := make([]opts.BarData, 0)
	for i := 0; i < 7; i++ {
		items = append(items, opts.BarData{Value: rand.Intn(300)})
	}
	return items
}

func main() {
	// create a new bar instance
	bar := charts.NewBar()
	// set some global options like Title/Legend/ToolTip or anything else
	bar.SetGlobalOptions(charts.WithTitleOpts(opts.Title{
		Title:    "My first bar chart generated by go-echarts",
		Subtitle: "It's extremely easy to use, right?",
	}))

	// Put data into instance
	bar.SetXAxis([]string{"Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"}).
		AddSeries("Category A", generateBarItems()).
		AddSeries("Category B", generateBarItems())
	// Where the magic happens
	f, _ := os.Create("bar.html")
	bar.Render(f)
}
```

`Render` 方法将会将渲染结果保存为 bar.html 文件

![](https://user-images.githubusercontent.com/19553554/98435974-5094f780-2112-11eb-81f6-b31d68f4d535.png)

当然也可以使用启动 http 服务将图表直接渲染出来

```golang
package main

import (
	"math/rand"
	"net/http"

	"github.com/go-echarts/go-echarts/v2/charts"
	"github.com/go-echarts/go-echarts/v2/opts"
	"github.com/go-echarts/go-echarts/v2/types"
)

// generate random data for line chart
func generateLineItems() []opts.LineData {
	items := make([]opts.LineData, 0)
	for i := 0; i < 7; i++ {
		items = append(items, opts.LineData{Value: rand.Intn(300)})
	}
	return items
}

func httpserver(w http.ResponseWriter, _ *http.Request) {
	// create a new line instance
	line := charts.NewLine()
	// set some global options like Title/Legend/ToolTip or anything else
	line.SetGlobalOptions(
		charts.WithInitializationOpts(opts.Initialization{Theme: types.ThemeWesteros}),
		charts.WithTitleOpts(opts.Title{
			Title:    "Line example in Westeros theme",
			Subtitle: "Line chart rendered by the http server this time",
		}))

	// Put data into instance
	line.SetXAxis([]string{"Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"}).
		AddSeries("Category A", generateLineItems()).
		AddSeries("Category B", generateLineItems()).
		SetSeriesOptions(charts.WithLineChartOpts(opts.LineChart{Smooth: opts.Bool(true)}))
	line.Render(w)
}

func main() {
	http.HandleFunc("/", httpserver)
	http.ListenAndServe(":8081", nil)
}
```

![image](https://user-images.githubusercontent.com/19553554/98436642-044cb600-2118-11eb-9f35-784948a92cb5.png)

### 🔖 Gallery

<div align="center">
<img src="https://user-images.githubusercontent.com/19553554/52197440-843a5200-289a-11e9-8601-3ce8d945b04a.gif" width="33%" alt="bar"/>
<img src="https://user-images.githubusercontent.com/19553554/52360729-ad640980-2a77-11e9-84e2-feff7e11aea5.gif" width="33%" alt="boxplot"/>
<img src="https://user-images.githubusercontent.com/19553554/52535290-4b611800-2d87-11e9-8bf2-b43a54a3bda8.png" width="33%" alt="effectScatter"/>
<img src="https://user-images.githubusercontent.com/19553554/52332816-ac5eb800-2a36-11e9-8227-3538976f447d.gif" width="33%" alt="funnel"/>
<img src="https://user-images.githubusercontent.com/19553554/52332988-0b243180-2a37-11e9-9db8-eb6b8c86a0de.png" width="33%" alt="gague"/>
<img src="https://user-images.githubusercontent.com/19553554/52344575-133f9980-2a56-11e9-93e0-568e484936ce.gif" width="33%" alt="geo"/>
<img src="https://user-images.githubusercontent.com/19553554/52727805-f7f20280-2ff0-11e9-91ab-cd99848e3127.gif" width="33%" alt="graph"/>
<img src="https://user-images.githubusercontent.com/19553554/52345115-6534ef00-2a57-11e9-80cd-9cbfed252139.gif" width="33%" alt="heatmap"/>
<img src="https://user-images.githubusercontent.com/19553554/52345490-4a16af00-2a58-11e9-9b43-7bbc86aa05b6.gif" width="33%" alt="kline"/>
<img src="https://user-images.githubusercontent.com/19553554/52346064-b7770f80-2a59-11e9-9e03-6dae3a8c637d.gif" width="33%" alt="line"/>
<img src="https://user-images.githubusercontent.com/19553554/52347117-248ba480-2a5c-11e9-8402-5a94054dca50.gif" width="33%" alt="liquid"/>
<img src="https://user-images.githubusercontent.com/19553554/52347915-0a52c600-2a5e-11e9-8039-41268238576c.gif" width="33%" alt="map"/>
<img src="https://user-images.githubusercontent.com/19553554/52535013-e48e2f80-2d83-11e9-8886-ac0d2122d6af.png" width="33%" alt="parallel"/>
<img src="https://user-images.githubusercontent.com/19553554/52348202-bb596080-2a5e-11e9-84a7-60732be0743a.gif" width="33%" alt="pie"/>
<img src="https://user-images.githubusercontent.com/19553554/52533994-932b7380-2d76-11e9-93b4-0de3132eb941.gif" width="33%" alt="radar"/>
<img src="https://user-images.githubusercontent.com/19553554/52348431-420e3d80-2a5f-11e9-8cab-7b415592dc77.gif" width="33%" alt="scatter"/>
<img src="https://user-images.githubusercontent.com/19553554/52348737-01fb8a80-2a60-11e9-94ac-dacbd7b58811.png" width="33%" alt="wordCloud"/>
<img src="https://user-images.githubusercontent.com/19553554/52433989-4f075b80-2b49-11e9-9979-ef32c2d17c96.gif" width="33%" alt="bar3D"/>
<img src="https://user-images.githubusercontent.com/19553554/52464826-4baab900-2bb7-11e9-8299-776f5ee43670.gif" width="33%" alt="line3D"/>
<img src="https://user-images.githubusercontent.com/19553554/52802261-8d0cfe00-30ba-11e9-8ae7-ae0773770a59.gif" width="33%" alt="sankey"/>
<img src="https://user-images.githubusercontent.com/19553554/52464647-aee81b80-2bb6-11e9-864e-c544392e523a.gif" width="33%" alt="scatter3D"/>
<img src="https://user-images.githubusercontent.com/19553554/52465183-a55fb300-2bb8-11e9-8c10-4519c4e3f758.gif" width="33%" alt="surface3D"/>
<img src="https://user-images.githubusercontent.com/19553554/52798246-7ebae400-30b2-11e9-8489-6c10339c3429.gif" width="33%" alt="themeRiver"/>
<img src="https://user-images.githubusercontent.com/19553554/52349544-c2ce3900-2a61-11e9-82af-28aaaaae0d67.gif" width="33%" alt="overlap"/>
</div>

### 📃 License

MIT [©go-echarts](https://github.com/go-echarts/go-echarts/blob/master/LICENSE)
