import 'dart:convert';
import 'dart:io';
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'package:intl/intl.dart';
void main() {
  runApp(const MyApp());
}
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Flutter Demo',
      theme: ThemeData(primarySwatch: Colors.amber),
      home: MyHomePage(title: '장바구니'),
    );
  }
}
class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key, required this.title}) : super(key: key);
  final String title;
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.black,
        title: Text(widget.title, style: const TextStyle(color: Colors.white)),
      ),
      body: Container(
        padding: EdgeInsets.all(5.0),
        child: Column(
          children:[
            _futureBuilder(),
            Container(
              padding: EdgeInsets.all(40.0),
              alignment: Alignment.centerRight,
              child: Column(
              crossAxisAlignment: CrossAxisAlignment.end,
                children:const [
                  Text("test"),
                  Text("test32"),
                ],
              )
            ),
          ]
        ),
      ),
    );
  }
  _futureBuilder(){
    return FutureBuilder(
        future: _getData(),
        builder: (context, snapshot) {
          if(snapshot.hasError  ) {
            Text(snapshot.error.toString());
          }
          if(snapshot.hasData){
            return _listView(snapshot);
          } else {
            return const Text("조회값이 없습니다.");
          }
        },
    );
  }
  _listView(AsyncSnapshot snapshot){
    return ListView.separated(
      separatorBuilder: (context, index) => Divider(color: Colors.grey),
      shrinkWrap: true,
      itemCount: snapshot.data?['cartList'].length,
      itemBuilder: (BuildContext context, int index) {
        return _dataListTileCell(DataModel.fromJson(snapshot.data['cartList'][index]));
       },
      );
  }
  Future _getData() async {
    String domain = "https://caspar.kr";
    String uri = "/church/교회";
    http.Response response;
    try {
      debugPrint(domain+uri);
      response = await http.get(Uri.parse(domain+uri),
      headers: {"Accept": "application/json"});
      var jsonData = jsonDecode(response.body);
      debugPrint(jsonData.toString());
      return jsonData;
    } catch(e) {
      debugPrint(e.toString());
      return jsonDecode('''{ "cartList":[{"id": 1,"title": "Apple 에어팟 프로", "price": 239000,"imageFile": "thumb_airpods.jpg"},
      {"id": 2,"title": "네스프레소 커피머신", "price": 134950,"imageFile": "thumb_nespresso.jpg"},
       {"id": 3,"title": "스타벅스 텀블러","price": 35000,"imageFile": "thumb_starbucks.jpg"}] }''');
    }
    //       return jsonDecode('''{ "cartList":[{"id": 1,"title": "Apple 에어팟 프로", "price": 239000,"imageFile": "thumb_airpods.jpg"},
    //   {"id": 2,"title": "네스프레소 커피머신", "price": 134950,"imageFile": "thumb_nespresso.jpg"},
    //    {"id": 3,"title": "스타벅스 텀블러","price": 35000,"imageFile": "thumb_starbucks.jpg"}] }''');
  }
  Widget _dataListTileCell(DataModel data) {
    var f =NumberFormat('###,###,###,###');
    return ListTile(
      leading : Image.asset(data.data2?? "", width: 80, height: 80,),
      title: Text(data.data1?? ""),
      trailing : Text(f.format(data.data3)),
      onTap: () =>     Navigator.of(context).push(
        CupertinoPageRoute<void>(
            builder: (BuildContext context) =>
                DataDetail(id: data.data4)
        ),
      ),
            );
  }
}
class DataModel {
  final dynamic? data1;
  final dynamic? data2;
  final dynamic? data3;
  final dynamic? data4;
  DataModel({
    this.data1,
    this.data2,
    this.data3,
    this.data4,
  });
  DataModel.fromJson(Map<String, dynamic> json) :
    data1 = json["title"],
    data2 = json["imageFile"],
    data3 = json["price"],
    data4 = json["id"];

  Map<String, dynamic> toJson() => {
    "data1": data1,
    "data2": data2,
    "data3": data3,
    "data4": data4,
  };
}
class DataDetail extends StatelessWidget {
  const DataDetail({Key? key,
  required this.id}) : super(key: key);
  final dynamic id;

  @override
  Widget build(BuildContext context){

    //var data = _getDetailData(id: id);
    var data = _getDetailData(id: 1);

    return Scaffold(
      appBar: AppBar(
        title: Text(data['item']['title']),
      ),
      body: Column(
        children: [
          Container(
            padding: EdgeInsets.all(10.0),
            child: Image.asset(
              data['item']['imageFile'],
              height: 200,
              width: 200,
            ),
          ),

          Container(
            padding: EdgeInsets.all(10.0),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.center,
              crossAxisAlignment: CrossAxisAlignment.end,
              children: [
                Container(
                  padding: EdgeInsets.all(10.0),
                  child: Column(
                    //mainAxisAlignment: MainAxisAlignment.center,
                    crossAxisAlignment: CrossAxisAlignment.end,
                    children: [
                      Text("판매가격", textAlign: TextAlign.right,),
                      Text("제조사", textAlign: TextAlign.right,),
                      Text("모텔명", textAlign: TextAlign.right,),
                    ],
                  ),

                ),
                Container(
                  padding: EdgeInsets.all(10),
                  child:Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(data['item']['price'].toString(), textAlign: TextAlign.left),
                      Text(data['item']['company'], textAlign: TextAlign.left,),
                      Text(data['item']['model'], textAlign: TextAlign.left,),
                    ]
                  )
                )
              ]
            ),
          )
      ],)
    );
  }

  _getDetailData({required int id}){
    return jsonDecode('''{"item":
    {"id": 1,"title": "Apple 에어팟 프로", "price": 239000, "imageFile": "thumb_airpods.jpg", "company": "APPLE", "model": "MWP22KH/A"} }''');
  }
}
