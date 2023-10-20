**※ Param을 이용한 값 받기**
```java
package com.controller;  
  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.servlet.ModelAndView;  
  
@Controller  
public class SumController {  
  
    //@RequestMapping(value = "input.do", method = RequestMethod.GET)  
    @GetMapping(value = "input.do")  
    public String input() {  
        return "sum/input";  
    }  
    //@RequestMapping(value = "result.do", method = RequestMethod.POST)  
    @PostMapping(value = "result.do")  
    public ModelAndView result(@RequestParam(required = false, defaultValue = "0") String x, @RequestParam(required = false, defaultValue = "0") String y) {  
        ModelAndView mav = new ModelAndView();  
        mav.addObject("x", x);  
        mav.addObject("y", y);  
        mav.setViewName("sum/result");  
  
        return mav;  
    }}
```

**※ Map을 이용한 값 받기**
```java
package com.controller;  
  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.servlet.ModelAndView;  
  
import java.util.Map;  
  
@Controller  
public class SumController {  
  
    //@RequestMapping(value = "input.do", method = RequestMethod.GET)  
    @GetMapping(value = "input.do")  
    public String input() {  
        return "sum/input";  
    }  
    //@RequestMapping(value = "result.do", method = RequestMethod.POST)  
    @PostMapping(value = "result.do")  
    public ModelAndView result(@RequestParam Map<String, String> map) {  
        ModelAndView mav = new ModelAndView();  
        mav.addObject("x", map.get(x));  
        mav.addObject("y", map.get(y));  
        mav.setViewName("sum/result");  
  
        return mav;  
    }}
```

**※ ModelMap을 이용한 값 받기**
```java
package com.controller;  
  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.ModelMap;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.servlet.ModelAndView;  
  
import javax.servlet.http.HttpSession;  
import java.util.Map;  
  
@Controller  
public class SumController {  
  
    //@RequestMapping(value = "input.do", method = RequestMethod.GET)  
    @GetMapping(value = "input.do")  
    public String input() {  
        return "sum/input";  
    }  
    //@RequestMapping(value = "result.do", method = RequestMethod.POST)  
    @PostMapping(value = "result.do")  
    public String result(@RequestParam Map<String, String> map, ModelMap modelMap, HttpSession session) {  
        modelMap.put("x", map.get("x"));  
        modelMap.put("y", map.get("y"));  
        return "sum/result";  
    }}
```

**※ DTO를 이용한 값 받기**
```java
package com.controller;  
  
import com.bean.SumDTO;  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.ModelMap;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.ModelAttribute;  
import org.springframework.web.bind.annotation.PostMapping;  
  
@Controller  
public class SumController {  
  
    //@RequestMapping(value = "input.do", method = RequestMethod.GET)  
    @GetMapping(value = "input.do")  
    public String input() {  
        return "sum/input";  
    }  
    //@RequestMapping(value = "result.do", method = RequestMethod.POST)  
    @PostMapping(value = "result.do")  
    public String result(@ModelAttribute SumDTO sumDTO, ModelMap modelMap) {  
        modelMap.put("x", sumDTO.getX());  
        modelMap.put("y", sumDTO.getY());  
        return "sum/result";  
    }}
```