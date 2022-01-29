# Java Project -- Web Inventory System
## Start From Environmental Setup
1. SpringBoot 2.6.3
2. PostgreSQL 14
3. Tomcat 9.0.56 embedded in SpringBoot
4. Eclipse 2021-12
6. Java 11

## Client Library WebJars
WebJars are client side dependencies packaged into JAR files, add following dependency
1. WebJars bootstrap 5.1.3
2. WebJars jquery 3.6.0

```xml
<dependency>
	<groupId>org.webjars</groupId>
	<artifactId>bootstrap</artifactId>
	<version>5.1.3</version>
</dependency>
<dependency>
	<groupId>org.webjars</groupId>
	<artifactId>jquery</artifactId>
	<version>3.6.0</version>
</dependency>
```

## Lombok installation in Eclipse
Lombok is a library that facilitates many tedious tasks and reduces Java source code verbosity<br>
1. Download Lombok jar file
2. Type "java -jar lombok-1.18.4.jar" in concole
3. Installation finish, then restart Eclipse
4. Project should be able to use @Data annotation now


# Step-By-Step Procedures

## Step 1: Create Spring project from Spring Intializr
Go to the [Spring Initializer](https://start.spring.io/)
- Choose "Maven Project", Language "Java" and Spring Boot version "2.6.3"
- Group: type "shop"
- Artifact: type “shopApp”
- Name: type “shopApp”
- Description: type any description
- Choose “Jar”, it will include embedded Tomcat server provided by Spring Boot
- Choose Java SDK 11

Add the following Dependencies
- Spring Web: required for RESTful web applications
- Spring Data JPA: required to access the data from the database. JPA (Java Persistence API) 
- PostgreSQL Driver: required to connect with PostgreSQL database
- Thymeleaf Driver: Thymeleaf is a Java-based library provides a good support for XHTML/HTML5 in web applications

<img width="1219" alt="Screenshot1" src="https://user-images.githubusercontent.com/48862763/151650813-c310bf0b-517a-49fc-80ff-fedfa662ed81.png">

Click the "Generate" button at the bottom of the screen, this will generate a project Zip file <br>
Then import project into Eclipse

## Step 2: Add sub-class to the project
 
- Repository: DAO(Data Access Object) layer which connects and accesses to the database
- Service: This layer calls the DAO and perform CRUD operations
- Model: The class mapping to the database table and provides getter and setter functions
- Controller: the class mapping to REST APIs controller for HTTP requests

### Step 2.1: Model class

```Java
package shop.shopApp.model;

import javax.persistence.*;
import lombok.Data;

@Entity
@Data
@Table(name="`inventory`")
public class shopModel {

	@Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name="id")
    private Long id;
	
	@Column(name="price")
    private Integer price;
    
    @Column(name="item")
    private String item;

}
```

### Step 2.2: Repository class

```Java
package shop.shopApp.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;
import shop.shopApp.model.shopModel;

@Repository
public interface shopRepository extends JpaRepository<shopModel, Long> {

	@Query(value="select * from inventory a where a.item = :item", nativeQuery=true)
    List<shopModel> getItem(String item);
}
```

### Step 2.3: Service class

```Java
package shop.shopApp.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import shop.shopApp.model.shopModel;
import shop.shopApp.repository.shopRepository;

@Service
public class shopService {
	@Autowired
	shopRepository rep;
	
	public shopModel saveItem(shopModel m) {
		return rep.save(m);
	}

	public void deleteItem(Long id) {
		rep.deleteById(id);
	}
	
	public List<shopModel> listAll() {
		return rep.findAll();
	}
	
	public shopModel listOneItem(Long id) {
		return rep.findById(id).get();
	}
	
	public void updateItem(String item, Integer price) {

		List<shopModel> ll = rep.getItem(item);
		for(shopModel s:ll) {
			s.setPrice(price);
			rep.save(s);
		}
		
	}
}
```

### Step 2.4: Controller class

```Java
package shop.shopApp.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import shop.shopApp.model.shopModel;
import shop.shopApp.service.shopService;

@Controller
public class shopController {
	
	@Autowired
	shopService service;
	
	@PostMapping("/add")
    public String postAdd(@ModelAttribute("shopModel") shopModel model) {
		service.saveItem(model);
		return "redirect:/index";
    }
	
	@GetMapping(value="/")
    public String GetDefault() {
        return "redirect:/index";
    }
    
    @GetMapping(value="/index")
    public String GetIndex(Model model, @ModelAttribute("shopModel") shopModel smodel) {
    	model.addAttribute("allItems", service.listAll());
        return "index";
    }

    @GetMapping(value="/showItem/{pid}")
    public String getShowItem(@PathVariable(value = "pid") Long id, Model model, @ModelAttribute("shopModel") shopModel smodel) {
    	shopModel cur = service.listOneItem(id);
    	model.addAttribute("shopModel", cur);
        return "update";
    }
    
    @PostMapping(value="/updateItem")
    public String postUpdateItem(@ModelAttribute("shopModel") shopModel smodel) {
    	service.updateItem(smodel.getItem(), smodel.getPrice());
        return "redirect:/index";
    }

    @GetMapping(value="/delete/{pid}")
    public String getDelete(@PathVariable(value = "pid") Long id) {
        service.deleteItem(id);
        return "redirect:/index";
    }
}

```
































