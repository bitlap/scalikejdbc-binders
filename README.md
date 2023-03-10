# scalikejdbc-binders

![CI][Badge-CI]  [![codecov][Badge-Codecov]][Link-Codecov]   [![Nexus (Snapshots)][Badge-Snapshots]][Link-Snapshots] 

[Badge-CI]: https://github.com/bitlap/scalikejdbc-binders/actions/workflows/ScalaCI.yml/badge.svg
[Badge-Codecov]: https://codecov.io/gh/bitlap/scalikejdbc-binders/branch/master/graph/badge.svg?token=IA596YRTOT
[Badge-Snapshots]: https://img.shields.io/nexus/s/org.bitlap/scalikejdbc-binders-postgres_3?server=https%3A%2F%2Fs01.oss.sonatype.org

[Link-Codecov]: https://codecov.io/gh/bitlap/scalikejdbc-binders
[Link-Snapshots]: https://s01.oss.sonatype.org/content/repositories/snapshots/org/bitlap/scalikejdbc-binders-postgres_3

----

# postgres

- Dependency

```scala
"org.bitlap" %% "scalikejdbc-binders-postgres" % <version>
```

- Inherit `ArrayBinders`
  - Supported `TypeBinder`
  - Supported `ParameterBinderFactory`
- Inherit `JsonBinders`
  - Supported `TypeBinder`
  - Supported `ParameterBinderFactory`

## ParameterBinderFactory array
| postgres type | scala type | supported collections |
|---------------|------------|-----------------------|
| varchar       | String     | List,Set,Seq,Vector   |
| integer       | Int        | List,Set,Seq,Vector   |
| decimal       | BigDecimal | List,Set,Seq,Vector   |
| bigint        | Long       | List,Set,Seq,Vector   |

## TypeBinder array
| postgres type | scala type | supported collections | required implicit mapping function |
|---------------|------------|-----------------------|------------------------------------|
| all           | all        | List,Set,Seq,Vector   | `Array[Any] => T[X]`               |

## ParameterBinderFactory json
| postgres type | scala type | required implicit mapping function |
|---------------|------------|------------------------------------|
| json          | `T`        | `String => T`                      |

## TypeBinder json
| postgres type | scala type | required implicit mapping function |
|---------------|------------|------------------------------------|
| json          | `T`        | `T => String`                      |

## manual definition
```scala
given ParameterBinderFactory[List[Short]] = DeriveParameterBinder.array[Short, List](OType.Short, _.toArray)

given ParameterBinderFactory[Map[String, String]] = DeriveParameterBinder.json[Map[String, String]](toJson)

given TypeBinder[List[BigDecimal]] = DeriveTypeBinder.array[BigDecimal, List](_.toList.map(s => BigDecimal(s.toString)), Nil)
```