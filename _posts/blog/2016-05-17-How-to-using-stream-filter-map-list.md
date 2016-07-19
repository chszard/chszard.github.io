---
layout: single
title: JAVA 8 How to using stream filter map in list by key
categories: blog
tags: [java8, stream, filter, ramda]
---

- Map 으로 이루어진 List에서 Map 의 key 로 특정 값들만 골라내는 예제.   

```java   
/**
* Created by chszard on 2016. 5. 17..
*/

import java.util.ArrayList;   
import java.util.HashMap;   
import java.util.List;   
import java.util.Map;   
import java.util.stream.Collectors;   

public class Main {
    public static void main(String[] args) {
        streamfilterlistMapExample();
    }

    public static void streamfilterlistMapExample(){
        List<Map<String, Object>> rows = new ArrayList<Map<String, Object>>();
        Map<String, Object> row1 = new HashMap<String, Object>();
        row1.put("idx", 1L);
        row1.put("title", "This is title");
        row1.put("flagYn", "Y");
        row1.put("created_at", "2016-05-17
        ");
        rows.add(row1);

        Map<String, Object> row2 = new HashMap<String, Object>();
        row2.put("idx", 2L);
        row2.put("title", "This is title2");
        row2.put("flagYn", "Y");
        row2.put("created_at", "2016-05-17");
        rows.add(row2);

        Map<String, Object> row3 = new HashMap<String, Object>();
        row3.put("idx", 3L);
        row3.put("title", "This is title3");
        row3.put("flagYn", "N");
        row3.put("created_at", "2016-05-17");
        rows.add(row3);

        List<Map<String, Object>> res = 
            rows.stream()
            .filter(map -> (map.get("flagYn")
            .equals("Y")))
        //  .forEach(System.out::println);
            z.collect(Collectors.toList());
        System.out.println(res);
        System.out.println(res.size());
        //System.out.println(rows);
    } 
}   
```