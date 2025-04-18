<p align="center">
	<img src="https://user-images.githubusercontent.com/19553554/52535979-c0d0e680-2d8f-11e9-85c8-2e9f659e7c6f.png" width=300 height=300 />
</p>

<h1 align="center">go-echarts</h1>
<p align="center">
    <em>🎨 The adorable charts library for Golang.</em>
</p>

<p align="center">
    <a href="https://github.com/go-echarts/go-echarts/actions/workflows/ci.yml" style="text-decoration: none;">
        <img src="https://github.com/go-echarts/go-echarts/actions/workflows/ci.yml/badge.svg" alt="Build Status">
    </a>
    <a href="https://goreportcard.com/report/github.com/go-echarts/go-echarts" style="text-decoration: none;">
        <img src="https://goreportcard.com/badge/github.com/go-echarts/go-echarts" alt="Go Report Card">
    </a>
	<a href="https://github.com/go-echarts/go-echarts/pulls" style="text-decoration: none;">
        <img src="https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat" alt="Contributions welcome">
    </a>
    <a href="https://opensource.org/licenses/MIT" style="text-decoration: none;">
        <img src="https://img.shields.io/badge/License-MIT-brightgreen.svg" alt="MIT License">
    </a>
    <a href="https://pkg.go.dev/github.com/go-echarts/go-echarts/v2" style="text-decoration: none;">
        <img src="https://godoc.org/github.com/go-echarts/go-echarts?status.svg" alt="GoDoc">
    </a>
    <a href="https://echarts.apache.org/" style="text-decoration: none;">
        <img src="https://img.shields.io/badge/echarts-_v5.4.3-orange" alt="echartsVersion">
    </a>
    <a href="https://go-echarts.github.io/go-echarts" style="text-decoration: none;">
        <img src="https://img.shields.io/badge/docs-_handbook-blue" alt="handbook">
    </a>
</p>

> If a language can be used to build web scrapers, it definitely needs to provide a graceful data visualization
> library. --- by dongdong.

In the Golang ecosystem, there are not many choices for data visualization libraries.  
The development of [go-echarts](https://github.com/go-echarts/go-echarts) aims to
provide a simple yet powerful data visualization library for Golang.   
[Apache ECharts](https://echarts.apache.org/) is an awesome charting and visualization library,
it supports adorable chart types and various interactive features.
and there have many program languages interactive with Echarts, such as [pyecharts](https://github.com/pyecharts/pyecharts),
which `go-echarts` learns and has evolved a lot from, and the [echarts4j](https://github.com/Koooooo-7/echarts4j) either.

[中文 README](README_CN.md)

### 🔰 Installation

Classic way to get go-echarts

```shell
# this may be not a good choice to use v2 go-echarts without gomod(GO111MODULE=off), the 
# next generation version management system 
# Technically, you could use go-echarts in the way below, if you have a better workaround, please let us know....

$ go get -u github.com/go-echarts/go-echarts/...
$ cd $go-echarts-project
$ mkdir v2 && mv charts components datasets opts render templates types v2
```

Use gomod style

```shell
$ go get -u github.com/go-echarts/go-echarts/v2/...
```

OR

```shell
# go.mod

require github.com/go-echarts/go-echarts/v2
```

### ⏳ Version

The go-echarts project is being developed under v2 version and the active codebase is on the master branch.

v1 and v2 are incompatible which means that you cannot upgrade go-echarts from v1 to v2 smoothly. But I think it is
worth trying that new version.

Especially, when there contains mino changes (usually in enhancement), we will release the `rc` version before a
standard release.
So, if you upgrade your projects cross the `rc` versions, maybe need little adjust, and I believe it is worthy to do.

### ✨ Features

* Clean and comprehensive API.
* Visualize your data in 25+ different ways.
* Highly configurable chart options.
* Detailed documentation and a rich collection of examples.
* Visualize your geographical data with 400+ maps.

### 📝 Usage

It's easy to get started with go-echarts with the [handbook](https://go-echarts.github.io/go-echarts), [go-echarts/examples](https://github.com/go-echarts/examples)
and [GoDocs](https://pkg.go.dev/github.com/go-echarts/go-echarts/v2).

In this example, we create a simple bar chart with only a few lines of code.

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

And the generated bar.html is rendered as below. Isn't that cool！

![](https://user-images.githubusercontent.com/19553554/98435974-5094f780-2112-11eb-81f6-b31d68f4d535.png)

Of course we can also start a listening web server with net/http.

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

For more information, please refer to [handbook](https://go-echarts.github.io/go-echarts/), [go-echarts/examples](https://github.com/go-echarts/examples) and
the [GoDoc](https://pkg.go.dev/github.com/go-echarts/go-echarts/v2).

### 💡 Contribution

go-echarts is an open source project and built on the top of other open-source projects.
Welcome all the kinds of contributions. No matter it is for typo fix, bug fix or big new features.
Please do not hesitate to ask a question or send a pull request.

We strongly value documentation and integration with other projects, so we are very glad to accept improvements for
these
aspects.

### ❤️ Contributors

![contributors](https://contrib.rocks/image?repo=go-echarts/go-echarts)

### 📃 License

MIT [©go-echarts](https://github.com/go-echarts/go-echarts/blob/master/LICENSE)
