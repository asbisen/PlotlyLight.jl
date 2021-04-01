# PlotlyLight

[![Build Status](https://travis-ci.com/joshday/PlotlyLight.jl.svg?branch=master)](https://travis-ci.com/joshday/PlotlyLight.jl)
[![Codecov](https://codecov.io/gh/joshday/PlotlyLight.jl/branch/master/graph/badge.svg)](https://codecov.io/gh/joshday/PlotlyLight.jl)


**PlotlyLight** is a low-level interface for working with [Plotly.js](https://plotly.com/javascript/),
an open source (MIT-licensed) plotting library. 

Everything is a pretty direct Julia-to-Javacript ([`EasyConfig.Config`](https://github.com/joshday/EasyConfig.jl) -> `JSON`) conversion.
 
## 🚀 Quickstart

```julia
using PlotlyLight 

p = Plot()

trace1 = Config()
trace1.x = 1:10
trace1.y = randn(10)

push!(p.data, trace1)

push!(p.data, Config(x=11:20, y=randn(10))) 

p.layout.title.text = "My Title"

p
```

**This won't display the plot in the REPL**.  Instead you'll see:

```julia
Plot
  Data
     trace 1: [:x, :y]
     trace 2: [:x, :y]
  Layout
     title: Config(:text => "My Title")
  Config
     displaylogo: false
```

- In environments like [Pluto.jl](https://github.com/fonsp/Pluto.jl), the plot **will** display.
- Instead, you can create an HTML div string via `repr("text/html", p)`.
- You can also create an HTML file string via `PlotlyLight.html(p)`.

## Custom Display Functions


### [DefaultApplication](https://github.com/tpapp/DefaultApplication.jl) (HTML)

```julia
using PlotlyLight, DefaultApplication

function f(p::Plot) 
    filename = joinpath(tempdir(), "temp.html")
    file = write(filename, PlotlyLight.html(p))
    DefaultApplication.open(filename)
end

p = Plot(Config(x = 1:10, y = randn(10)))

f(p)
```

### [Blink.jl](https://github.com/JuliaGizmos/Blink.jl)

```julia
using Blink, PlotlyLight

w = Window()

load!(w, "https://cdn.plot.ly/plotly-latest.min.js")

f(p) = body!(w, p)

f(Plot(Config(x = 1:10, y = randn(10))))
```