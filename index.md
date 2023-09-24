# ${Typesafe} %Config .Ops

[![codecov](https://codecov.io/gh/typesafeconfigops/TypesafeConfigOps/branch/master/graph/badge.svg)](https://codecov.io/gh/typesafeconfigops/TypesafeConfigOps)
[![Build Status](https://travis-ci.org/typesafeconfigops/TypesafeConfigOps.svg?branch=master)](https://travis-ci.org/typesafeconfigops/TypesafeConfigOps)

### Source code
[TypesafeConfigOps](https://github.com/typesafeconfigops/TypesafeConfigOps)

### Usage
```scala
// build.sbt
libraryDependencies += "io.github.typesafeconfigops" %% "typesafe-config-ops" % "3.0"
```

### ConfigOptOps
Import `TypesafeConfigOps.*` or `ConfigOptOps.*` allows to extract optional values from configuration.

```scala
import TypesafeConfigOps.ConfigOptOps

private val cfg = ConfigFactory.parseString("""{ "i" = 1 }""")
cfg.optInt("i") // Some(1)
cfg.optInt("ix") // None
```

### ConfigDefaultOps
Import `TypesafeConfigOps.*` or `ConfigDefaultOps.*` allows using default values for non-existing paths in configuration.

```scala
import TypesafeConfigOps.ConfigDefaultOps

private val cfg = ConfigFactory.parseString("""{ "i" = 1 }""")
cfg.getInt("i", 2) // 1
cfg.getInt("ix", 2) // 2
```
### ConfigTemplateOps
Import `TypesafeConfigOps.*` or `ConfigTemplateOps.*` allows to work with templates in configuration.

```scala
import TypesafeConfigOps.ConfigTemplateOps

val cfg = ConfigFactory.parseString(
  """
    |{
    |  code = <img ${imgSrc} ${imgHeight} ${imgWidth} ${imgBorder} />
    |}
  """.stripMargin)

val code = cfg
  .resolveTemplate(
    "imgSrc" -> "src='%s'",
    "imgHeight" -> "height='%dpx'",
    "imgWidth" -> "width='%dpx'",
    "imgBorder" -> "border='#%X%X%X'")
  .formatTemplate("code", "https://google.com/logo.png", 100, 200, 255, 255, 255)

code // "<img src='https://google.com/logo.png' height='100px' width='200px' border='#FFFFFF' />"
```
