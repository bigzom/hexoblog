---
title: xml
date: 2019-08-19 19:10:50
tags: [xml,dtd,dom4j]
---
##### 模板
- 导入外部DTD
`<!DOCTYPE note SYSTEM "note.dtd">`
<!-- more -->
- 内部DTD
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!-- 定义DTD -->
  <!-- 定义属性 <!ATTLIST 元素名称 属性名称 属性类型 默认值> -->
  <!DOCTYPE note [
          <!ELEMENT note (to,from,heading,body)>
          <!ELEMENT to      (#PCDATA)>
          <!ELEMENT from    (#PCDATA)>
          <!ELEMENT heading (#PCDATA)>
          <!ELEMENT body    (#PCDATA)>
          <!ATTLIST to type CDATA #REQUIRED >
          ]>
  <note>
      <to type="ss">George</to>
      <from>John</from>
      <heading>Reminder</heading>
      <body>Don't forget the meeting!</body>
  </note>
  ```
- java通过dom4j读取xml
[dom4j下载地址](https://dom4j.github.io"). 
  ```java
  public class ReadXml {
      public static void main(String[] args) throws DocumentException {
          //创建SAXReader读取xml
          SAXReader reader = new SAXReader();
          //读取xml
          Document document = reader.read(new File("src/test.xml"));
          //获取根元素
          Element rootElement = document.getRootElement();
          //获取根元素名字
          System.out.println(rootElement.getName());
          //获取根元素下所有子元素
          Iterator<Element> element = rootElement.elementIterator();
          //取出元素
          while (element.hasNext()){
              Element e = element.next();
            /*  //获取子元素的子元素
              Iterator<Element> el = e.elementIterator();
              while (el.hasNext()){
                  System.out.println("     "+el.next().getName());
              }*/
              //获得名字
              System.out.println(e.getName()+":"+e.getText());
              //获取属性 也可以用迭代器  e.attributeIterator()
              Attribute type = e.attribute("type");
              if(type != null){
              System.out.println("    "+type.getName()+"="+type.getValue());}
          }
      }
  }
  ```
- java通过dom4j写入xml
  ``` java
  public class WriteXml {
      public static void main(String[] args) throws IOException {
          //生成一个Document对象
          Document doc = DocumentHelper.createDocument();
          //添加并得到根元素
          Element root = doc.addElement("books");
          //为根元素添加子元素
          Element book = root.addElement("book");
          //为book添加属性,返回还是book这个对象，方便链式编程
          book.addAttribute("id","1");
          //为book添加子元素
          Element name = book.addElement("name");
          Element author = book.addElement("author");
          Element price = book.addElement("price");
          //为子元素添加文本
          name.addText("thinking in java");
          author.addText("John");
          price.addText("76");
        /*  //输出Document到xml中
        Writer writer = new FileWriter("src/book2.xml");
        doc.write(writer);
        writer.close();*/
       //格式良好的输出
        OutputFormat format = OutputFormat.createPrettyPrint();
        XMLWriter writer = new XMLWriter(new FileWriter(new File("src/book")), format);
        writer.write(doc);
        //关闭资源
        writer.close();
    }
  }
  ```