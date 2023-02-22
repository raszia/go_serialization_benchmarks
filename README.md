# Benchmarks of Go serialization methods

[![Gitter chat](https://badges.gitter.im/alecthomas.png)](https://gitter.im/alecthomas/Lobby)

This is a test suite for benchmarking various Go serialization methods.

## Tested serialization methods

- [encoding/gob](http://golang.org/pkg/encoding/gob/)
- [encoding/json](http://golang.org/pkg/encoding/json/)
- [github.com/json-iterator/go](https://github.com/json-iterator/go)
- [github.com/alecthomas/binary](https://github.com/alecthomas/binary)
- [github.com/davecgh/go-xdr/xdr](https://github.com/davecgh/go-xdr)
- [github.com/Sereal/Sereal/Go/sereal](https://github.com/Sereal/Sereal)
- [github.com/ugorji/go/codec](https://github.com/ugorji/go/tree/master/codec)
- [github.com/vmihailenco/msgpack/v4](https://github.com/vmihailenco/msgpack)
- [labix.org/v2/mgo/bson](https://labix.org/v2/mgo/bson)
- [github.com/tinylib/msgp](https://github.com/tinylib/msgp) *(code generator for msgpack)*
- [google.golang.org/protobuf](https://google.golang.org/protobuf) (generated code)
- [github.com/gogo/protobuf](https://github.com/gogo/protobuf) (generated code, optimized version of `goprotobuf`)
- [go.dedis.ch/protobuf](https://go.dedis.ch/protobuf) (reflection based)
- [github.com/google/flatbuffers](https://github.com/google/flatbuffers)
- [github.com/hprose/hprose-go/io](https://github.com/hprose/hprose-go)
- [github.com/glycerine/go-capnproto](https://github.com/glycerine/go-capnproto)
- [zombiezen.com/go/capnproto2](https://godoc.org/zombiezen.com/go/capnproto2)
- [github.com/andyleap/gencode](https://github.com/andyleap/gencode)
- [github.com/pascaldekloe/colfer](https://github.com/pascaldekloe/colfer)
- [github.com/linkedin/goavro](https://github.com/linkedin/goavro)
- [github.com/ikkerens/ikeapack](https://github.com/ikkerens/ikeapack)
- [github.com/raszia/gotiny](https://github.com/raszia/gotiny)
- [github.com/prysmaticlabs/go-ssz](https://github.com/prysmaticlabs/go-ssz)
- [github.com/200sc/bebop](https://github.com/200sc/bebop) (generated code)
- [github.com/shamaton/msgpackgen](https://github.com/shamaton/msgpackgen) (generated code)
- [github.com/ymz-ncnk/musgo](https://github.com/ymz-ncnk/musgo) (generated code)

## Running the benchmarks

```bash
go get -u -t
go test -bench='.*' ./
```

To update the table in the README:

```bash
./stats.sh
```

## Recommendation

If performance, correctness and interoperability are the most
important factors, [gogoprotobuf](https://gogo.github.io/) is
currently the best choice. It does require a pre-processing step (eg.
via Go 1.4's "go generate" command).

But as always, make your own choice based on your requirements.

## Data

The data being serialized is the following structure with randomly generated values:

```go
type A struct {
    Name     string
    BirthDay time.Time
    Phone    string
    Siblings int
    Spouse   bool
    Money    float64
}
```

## Results

Results with Go 1.20 linux/amd64 on an `Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz`

benchmark                                        | iter       | time/iter    | bytes/op  | allocs/op  | tt.sec | tt.kb        | ns/alloc
-------------------------------------------------|------------|--------------|-----------|------------|--------|--------------|-----------
Benchmark_Gotiny_Marshal-8                       |    3289964 |    314 ns/op |        48 |        168 |   1.03 |        15791 |    1.87
Benchmark_Gotiny_Unmarshal-8                     |    5535879 |    203 ns/op |        48 |        112 |   1.12 |        26572 |    1.81
Benchmark_GotinyNoTime_Marshal-8                 |    3071874 |    356 ns/op |        48 |        168 |   1.09 |        14744 |    2.12
Benchmark_GotinyNoTime_Unmarshal-8               |    5650100 |    196 ns/op |        47 |         96 |   1.11 |        27114 |    2.05
Benchmark_Msgp_Marshal-8                         |    8620024 |    129 ns/op |        97 |        128 |   1.12 |        83614 |    1.01
Benchmark_Msgp_Unmarshal-8                       |    5248064 |    225 ns/op |        97 |        112 |   1.18 |        50906 |    2.01
Benchmark_VmihailencoMsgpack_Marshal-8           |    1663527 |    641 ns/op |        92 |        264 |   1.07 |        15304 |    2.43
Benchmark_VmihailencoMsgpack_Unmarshal-8         |    1256986 |    940 ns/op |        92 |        160 |   1.18 |        11564 |    5.88
Benchmark_Json_Marshal-8                         |    1000000 |   1031 ns/op |       151 |        208 |   1.03 |        15160 |    4.96
Benchmark_Json_Unmarshal-8                       |     488434 |   2374 ns/op |       151 |        351 |   1.16 |         7409 |    6.76
Benchmark_JsonIter_Marshal-8                     |    1681933 |    830 ns/op |       141 |        200 |   1.40 |        23765 |    4.15
Benchmark_JsonIter_Unmarshal-8                   |    1000000 |   1140 ns/op |       141 |        216 |   1.14 |        14130 |    5.28
Benchmark_EasyJson_Marshal-8                     |     941330 |   1069 ns/op |       151 |        896 |   1.01 |        14270 |    1.19
Benchmark_EasyJson_Unmarshal-8                   |    1000000 |   1002 ns/op |       151 |        112 |   1.00 |        15160 |    8.95
Benchmark_Bson_Marshal-8                         |    1247524 |   1046 ns/op |       110 |        376 |   1.30 |        13722 |    2.78
Benchmark_Bson_Unmarshal-8                       |     648896 |   1609 ns/op |       110 |        224 |   1.04 |         7137 |    7.18
Benchmark_MongoBson_Marshal-8                    |     776659 |   1541 ns/op |       110 |        240 |   1.20 |         8543 |    6.42
Benchmark_MongoBson_Unmarshal-8                  |     711096 |   1671 ns/op |       110 |        408 |   1.19 |         7822 |    4.10
Benchmark_Gob_Marshal-8                          |     249010 |   4790 ns/op |       163 |       1616 |   1.19 |         4073 |    2.96
Benchmark_Gob_Unmarshal-8                        |      45433 |  30266 ns/op |       163 |       7768 |   1.38 |          743 |    3.90
Benchmark_XDR_Marshal-8                          |     811070 |   1409 ns/op |        92 |        440 |   1.14 |         7461 |    3.20
Benchmark_XDR_Unmarshal-8                        |    1041231 |   1134 ns/op |        91 |        231 |   1.18 |         9578 |    4.91
Benchmark_UgorjiCodecMsgpack_Marshal-8           |    1297413 |    946 ns/op |        91 |       1240 |   1.23 |        11806 |    0.76
Benchmark_UgorjiCodecMsgpack_Unmarshal-8         |    1090250 |   1072 ns/op |        91 |        688 |   1.17 |         9921 |    1.56
Benchmark_UgorjiCodecBinc_Marshal-8              |    1000000 |   1075 ns/op |        95 |       1256 |   1.07 |         9500 |    0.86
Benchmark_UgorjiCodecBinc_Unmarshal-8            |     825602 |   1225 ns/op |        95 |        848 |   1.01 |         7843 |    1.44
Benchmark_Sereal_Marshal-8                       |     501624 |   2348 ns/op |       132 |        832 |   1.18 |         6621 |    2.82
Benchmark_Sereal_Unmarshal-8                     |     357580 |   2838 ns/op |       132 |        976 |   1.01 |         4720 |    2.91
Benchmark_Binary_Marshal-8                       |     602824 |   1684 ns/op |        61 |        360 |   1.02 |         3677 |    4.68
Benchmark_Binary_Unmarshal-8                     |     854793 |   1379 ns/op |        61 |        320 |   1.18 |         5214 |    4.31
Benchmark_FlatBuffers_Marshal-8                  |    1201532 |    913 ns/op |        95 |        376 |   1.10 |        11445 |    2.43
Benchmark_FlatBuffers_Unmarshal-8                |    4790798 |    246 ns/op |        95 |        112 |   1.18 |        45555 |    2.20
Benchmark_CapNProto_Marshal-8                    |     591126 |   1883 ns/op |        96 |       4392 |   1.11 |         5674 |    0.43
Benchmark_CapNProto_Unmarshal-8                  |    2406106 |    471 ns/op |        96 |        192 |   1.13 |        23098 |    2.46
Benchmark_CapNProto2_Marshal-8                   |     915288 |   1132 ns/op |        96 |       1452 |   1.04 |         8786 |    0.78
Benchmark_CapNProto2_Unmarshal-8                 |    2153727 |    501 ns/op |        96 |        272 |   1.08 |        20675 |    1.84
Benchmark_Hprose_Marshal-8                       |    1000000 |   1063 ns/op |        85 |        412 |   1.06 |         8525 |    2.58
Benchmark_Hprose_Unmarshal-8                     |    1115200 |   1041 ns/op |        85 |        304 |   1.16 |         9507 |    3.42
Benchmark_Hprose2_Marshal-8                      |    2596594 |    436 ns/op |        85 |          0 |   1.13 |        22151 |    0.00
Benchmark_Hprose2_Unmarshal-8                    |    2252002 |    526 ns/op |        85 |        136 |   1.18 |        19196 |    3.87
Benchmark_Protobuf_Marshal-8                     |    1770528 |    665 ns/op |        52 |        144 |   1.18 |         9206 |    4.62
Benchmark_Protobuf_Unmarshal-8                   |    1337763 |    867 ns/op |        52 |        184 |   1.16 |         6956 |    4.71
Benchmark_Gogoprotobuf_Marshal-8                 |    9092077 |    126 ns/op |        53 |         64 |   1.15 |        48188 |    1.98
Benchmark_Gogoprotobuf_Unmarshal-8               |    5161258 |    222 ns/op |        53 |         96 |   1.15 |        27354 |    2.31
Benchmark_Gogojsonpb_Marshal-8                   |     104774 |  10614 ns/op |       126 |       3111 |   1.11 |         1321 |    3.41
Benchmark_Gogojsonpb_Unmarshal-8                 |      85020 |  13541 ns/op |       126 |       3383 |   1.15 |         1072 |    4.00
Benchmark_Colfer_Marshal-8                       |    9224456 |    121 ns/op |        51 |         64 |   1.12 |        47100 |    1.90
Benchmark_Colfer_Unmarshal-8                     |    6255799 |    185 ns/op |        52 |        112 |   1.16 |        32530 |    1.66
Benchmark_Gencode_Marshal-8                      |    7347620 |    154 ns/op |        53 |         80 |   1.13 |        38942 |    1.93
Benchmark_Gencode_Unmarshal-8                    |    6154107 |    183 ns/op |        53 |        112 |   1.13 |        32616 |    1.63
Benchmark_GencodeUnsafe_Marshal-8                |   11613321 |     94 ns/op |        46 |         48 |   1.10 |        53421 |    1.97
Benchmark_GencodeUnsafe_Unmarshal-8              |    7445767 |    153 ns/op |        46 |         96 |   1.14 |        34250 |    1.60
Benchmark_XDR2_Marshal-8                         |    7233091 |    152 ns/op |        60 |         64 |   1.10 |        43398 |    2.39
Benchmark_XDR2_Unmarshal-8                       |    9237249 |    118 ns/op |        60 |         32 |   1.09 |        55423 |    3.70
Benchmark_GoAvro_Marshal-8                       |     450452 |   2340 ns/op |        47 |        728 |   1.05 |         2117 |    3.21
Benchmark_GoAvro_Unmarshal-8                     |     199564 |   5578 ns/op |        47 |       2544 |   1.11 |          937 |    2.19
Benchmark_GoAvro2Text_Marshal-8                  |     361972 |   2971 ns/op |       133 |       1320 |   1.08 |         4843 |    2.25
Benchmark_GoAvro2Text_Unmarshal-8                |     393451 |   2777 ns/op |       133 |        736 |   1.09 |         5264 |    3.77
Benchmark_GoAvro2Binary_Marshal-8                |    1401618 |    848 ns/op |        47 |        464 |   1.19 |         6587 |    1.83
Benchmark_GoAvro2Binary_Unmarshal-8              |    1282917 |    941 ns/op |        47 |        544 |   1.21 |         6029 |    1.73
Benchmark_Ikea_Marshal-8                         |    1221538 |   1160 ns/op |        55 |        184 |   1.42 |         6718 |    6.30
Benchmark_Ikea_Unmarshal-8                       |    1458270 |    929 ns/op |        55 |        160 |   1.36 |         8020 |    5.81
Benchmark_ShamatonMapMsgpack_Marshal-8           |    1667377 |    700 ns/op |        92 |        192 |   1.17 |        15339 |    3.65
Benchmark_ShamatonMapMsgpack_Unmarshal-8         |    1787547 |    733 ns/op |        92 |        168 |   1.31 |        16445 |    4.37
Benchmark_ShamatonArrayMsgpack_Marshal-8         |    1944530 |    620 ns/op |        50 |        160 |   1.21 |         9722 |    3.88
Benchmark_ShamatonArrayMsgpack_Unmarshal-8       |    2137424 |    557 ns/op |        50 |        168 |   1.19 |        10687 |    3.32
Benchmark_ShamatonMapMsgpackgen_Marshal-8        |    5579025 |    210 ns/op |        92 |         96 |   1.17 |        51327 |    2.19
Benchmark_ShamatonMapMsgpackgen_Unmarshal-8      |    3038484 |    393 ns/op |        92 |        112 |   1.19 |        27954 |    3.51
Benchmark_ShamatonArrayMsgpackgen_Marshal-8      |    7379319 |    157 ns/op |        50 |         64 |   1.16 |        36896 |    2.46
Benchmark_ShamatonArrayMsgpackgen_Unmarshal-8    |    5105184 |    289 ns/op |        50 |        112 |   1.48 |        25525 |    2.58
Benchmark_SSZNoTimeNoStringNoFloatA_Marshal-8    |     190930 |   5616 ns/op |        55 |        440 |   1.07 |         1050 |   12.76
Benchmark_SSZNoTimeNoStringNoFloatA_Unmarshal-8  |     144256 |   7495 ns/op |        55 |       1184 |   1.08 |          793 |    6.33
Benchmark_Bebop_Marshal-8                        |    8837412 |    130 ns/op |        55 |         64 |   1.16 |        48605 |    2.05
Benchmark_Bebop_Unmarshal-8                      |   10519754 |    113 ns/op |        55 |         32 |   1.20 |        57858 |    3.55
Benchmark_FastJson_Marshal-8                     |    1391612 |    771 ns/op |       133 |        504 |   1.07 |        18619 |    1.53
Benchmark_FastJson_Unmarshal-8                   |     616030 |   1898 ns/op |       133 |       1704 |   1.17 |         8242 |    1.11
Benchmark_Musgo_Marshal-8                        |    9924801 |    108 ns/op |        46 |         48 |   1.08 |        45654 |    2.26
Benchmark_Musgo_Unmarshal-8                      |    6210996 |    182 ns/op |        46 |         96 |   1.13 |        28570 |    1.90
Benchmark_MusgoUnsafe_Marshal-8                  |   11188216 |    100 ns/op |        46 |         48 |   1.13 |        51465 |    2.10
Benchmark_MusgoUnsafe_Unmarshal-8                |    9953605 |    112 ns/op |        46 |         64 |   1.12 |        45786 |    1.76

Totals:

benchmark                               | iter       | time/iter    | bytes/op  | allocs/op  | tt.sec | tt.kb        | ns/alloc
----------------------------------------|------------|--------------|-----------|------------|--------|--------------|-----------
Benchmark_MusgoUnsafe_-8                |   21141821 |    213 ns/op |        92 |        112 |   4.51 |       194504 |    1.91
Benchmark_Bebop_-8                      |   19357166 |    244 ns/op |       110 |         96 |   4.73 |       212928 |    2.55
Benchmark_GencodeUnsafe_-8              |   19059088 |    248 ns/op |        92 |        144 |   4.73 |       175343 |    1.72
Benchmark_XDR2_-8                       |   16470340 |    271 ns/op |       120 |         96 |   4.46 |       197644 |    2.82
Benchmark_Musgo_-8                      |   16135797 |    290 ns/op |        92 |        144 |   4.69 |       148449 |    2.02
Benchmark_Colfer_-8                     |   15480255 |    307 ns/op |       103 |        176 |   4.76 |       159539 |    1.75
Benchmark_Gencode_-8                    |   13501727 |    337 ns/op |       106 |        192 |   4.55 |       143118 |    1.76
Benchmark_Gogoprotobuf_-8               |   14253335 |    349 ns/op |       106 |        160 |   4.98 |       151085 |    2.18
Benchmark_Msgp_-8                       |   13868088 |    355 ns/op |       194 |        240 |   4.92 |       269040 |    1.48
Benchmark_ShamatonArrayMsgpackgen_-8    |   12484503 |    447 ns/op |       100 |        176 |   5.58 |       124845 |    2.54
Benchmark_Gotiny_-8                     |    8825843 |    517 ns/op |        96 |        280 |   4.57 |        84728 |    1.85
Benchmark_GotinyNoTime_-8               |    8721974 |    553 ns/op |        95 |        264 |   4.82 |        83722 |    2.09
Benchmark_ShamatonMapMsgpackgen_-8      |    8617509 |    603 ns/op |       184 |        208 |   5.20 |       158562 |    2.90
Benchmark_Hprose2_-8                    |    4848596 |    962 ns/op |       170 |        136 |   4.67 |        82692 |    7.08
Benchmark_FlatBuffers_-8                |    5992330 |   1160 ns/op |       190 |        488 |   6.95 |       114064 |    2.38
Benchmark_ShamatonArrayMsgpack_-8       |    4081954 |   1177 ns/op |       100 |        328 |   4.81 |        40819 |    3.59
Benchmark_ShamatonMapMsgpack_-8         |    3454924 |   1434 ns/op |       184 |        360 |   4.96 |        63570 |    3.98
Benchmark_Protobuf_-8                   |    3108291 |   1533 ns/op |       104 |        328 |   4.77 |        32326 |    4.67
Benchmark_VmihailencoMsgpack_-8         |    2920513 |   1581 ns/op |       184 |        424 |   4.62 |        53737 |    3.73
Benchmark_CapNProto2_-8                 |    3069015 |   1633 ns/op |       192 |       1724 |   5.01 |        58925 |    0.95
Benchmark_GoAvro2Binary_-8              |    2684535 |   1789 ns/op |        94 |       1008 |   4.80 |        25234 |    1.78
Benchmark_JsonIter_-8                   |    2681933 |   1970 ns/op |       282 |        416 |   5.29 |        75791 |    4.74
Benchmark_UgorjiCodecMsgpack_-8         |    2387663 |   2018 ns/op |       182 |       1928 |   4.82 |        43455 |    1.05
Benchmark_EasyJson_-8                   |    1941330 |   2071 ns/op |       303 |       1008 |   4.02 |        58861 |    2.05
Benchmark_Ikea_-8                       |    2679808 |   2089 ns/op |       110 |        344 |   5.60 |        29477 |    6.07
Benchmark_Hprose_-8                     |    2115200 |   2104 ns/op |       170 |        716 |   4.45 |        36064 |    2.94
Benchmark_UgorjiCodecBinc_-8            |    1825602 |   2300 ns/op |       190 |       2104 |   4.20 |        34686 |    1.09
Benchmark_CapNProto_-8                  |    2997232 |   2354 ns/op |       192 |       4584 |   7.06 |        57546 |    0.51
Benchmark_XDR_-8                        |    1852301 |   2543 ns/op |       183 |        671 |   4.71 |        34080 |    3.79
Benchmark_Bson_-8                       |    1896420 |   2655 ns/op |       220 |        600 |   5.03 |        41721 |    4.42
Benchmark_FastJson_-8                   |    2007642 |   2669 ns/op |       267 |       2208 |   5.36 |        53724 |    1.21
Benchmark_Binary_-8                     |    1457617 |   3063 ns/op |       122 |        680 |   4.46 |        17782 |    4.50
Benchmark_MongoBson_-8                  |    1487755 |   3212 ns/op |       220 |        648 |   4.78 |        32730 |    4.96
Benchmark_Json_-8                       |    1488434 |   3405 ns/op |       303 |        559 |   5.07 |        45144 |    6.09
Benchmark_Sereal_-8                     |     859204 |   5186 ns/op |       264 |       1808 |   4.46 |        22682 |    2.87
Benchmark_GoAvro2Text_-8                |     755423 |   5748 ns/op |       267 |       2056 |   4.34 |        20215 |    2.80
Benchmark_GoAvro_-8                     |     650016 |   7918 ns/op |        94 |       3272 |   5.15 |         6110 |    2.42
Benchmark_SSZNoTimeNoStringNoFloatA_-8  |     335186 |  13111 ns/op |       110 |       1624 |   4.39 |         3687 |    8.07
Benchmark_Gogojsonpb_-8                 |     189794 |  24155 ns/op |       252 |       6494 |   4.58 |         4786 |    3.72
Benchmark_Gob_-8                        |     294443 |  35056 ns/op |       327 |       9384 |  10.32 |         9634 |    3.74

## Issues

The benchmarks can also be run with validation enabled.

```bash
VALIDATE=1 go test -bench='.*' ./
```

Unfortunately, several of the serializers exhibit issues:

1. **(minor)** BSON drops sub-microsecond precision from `time.Time`.
2. **(minor)** Ugorji Binc Codec drops the timezone name (eg. "EST" -> "-0500") from `time.Time`.

```
--- FAIL: BenchmarkBsonUnmarshal-8
    serialization_benchmarks_test.go:115: unmarshaled object differed:
        &{20b999e3621bd773 2016-01-19 14:05:02.469416459 -0800 PST f017c8e9de 4 true 0.20887343719329818}
        &{20b999e3621bd773 2016-01-19 14:05:02.469 -0800 PST f017c8e9de 4 true 0.20887343719329818}
--- FAIL: BenchmarkUgorjiCodecBincUnmarshal-8
    serialization_benchmarks_test.go:115: unmarshaled object differed:
        &{20a1757ced6b488e 2016-01-19 14:05:15.69474534 -0800 PST 71f3bf4233 0 false 0.8712180830484527}
        &{20a1757ced6b488e 2016-01-19 14:05:15.69474534 -0800 -0800 71f3bf4233 0 false 0.8712180830484527}
```

All other fields are correct however.

Additionally, while not a correctness issue, FlatBuffers, ProtoBuffers, Cap'N'Proto and ikeapack do not
support time types directly. In the benchmarks an int64 value is used to hold a UnixNano timestamp.

3. **(major)** Goprotobuf has been disabled in the above, as it is no longer maintained and is incompatible with the latest changes to Google's Protobuf package. See discussions: [1](https://github.com/containerd/ttrpc/issues/62), [2](https://github.com/containerd/ttrpc/pull/99).

```
panic: protobuf tag not enough fields in ProtoBufA.state:

goroutine 148 [running]:
github.com/gogo/protobuf/proto.(*unmarshalInfo).computeUnmarshalInfo(0xc0000d9c20)
 /home/ken/src/go/pkg/mod/github.com/gogo/protobuf@v1.3.2/proto/table_unmarshal.go:341 +0x135f
github.com/gogo/protobuf/proto.(*unmarshalInfo).unmarshal(0xc0000d9c20, {0xcff820?}, {0xc000122fc0, 0x35, 0x35})
 /home/ken/src/go/pkg/mod/github.com/gogo/protobuf@v1.3.2/proto/table_unmarshal.go:138 +0x67
github.com/gogo/protobuf/proto.(*InternalMessageInfo).Unmarshal(0xc00013a8c0?, {0xe6d5f0, 0xc000114060}, {0xc000122fc0?, 0x35?, 0x35?})
 /home/ken/src/go/pkg/mod/github.com/gogo/protobuf@v1.3.2/proto/table_unmarshal.go:63 +0xd0
github.com/gogo/protobuf/proto.(*Buffer).Unmarshal(0xc00009fe28, {0xe6d5f0, 0xc000114060})
 /home/ken/src/go/pkg/mod/github.com/gogo/protobuf@v1.3.2/proto/decode.go:424 +0x153
github.com/gogo/protobuf/proto.Unmarshal({0xc000122fc0, 0x35, 0x35}, {0xe6d5f0, 0xc000114060})
 /home/ken/src/go/pkg/mod/github.com/gogo/protobuf@v1.3.2/proto/decode.go:342 +0xe6
github.com/alecthomas/go_serialization_benchmarks.Benchmark_Goprotobuf_Unmarshal(0xc0000d5680)
 /home/ken/src/go/src/github.com/alecthomas/go_serialization_benchmarks/serialization_benchmarks_test.go:853 +0x26a
testing.(*B).runN(0xc0000d5680, 0x1)
 /usr/local/go/src/testing/benchmark.go:193 +0x102
testing.(*B).run1.func1()
 /usr/local/go/src/testing/benchmark.go:233 +0x59
created by testing.(*B).run1
 /usr/local/go/src/testing/benchmark.go:226 +0x9c
exit status 2
FAIL github.com/alecthomas/go_serialization_benchmarks 61.159s
```
