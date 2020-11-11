## gRPC

## Simple
1. .proto 확장자 파일을 작성
2. 작성된 파일을 프로토버프 컴파일러를 통해 소스코드 생성
3. 만들어진 추상클래스(소스코드)를 상속받아 메서드 오버라이딩으로 구현


## Detail

### gRPC message & service Definition
```proto
message Point {
  int32 latitude = 1;
  int32 longitude = 2;
}

message Feature {
  ...
}

message Rectangle {
  ...
}

service RouteGuide {

  rpc GetFeature(Point) returns (Feature) {}

  rpc ListFeatures(Rectangle) returns (stream Feature) {}
}
```

### Source generate
```groovy

apply plugin: 'com.google.protobuf'
 
buildscript { 
    repositories { 
        mavenCentral() 
    }
dependencies { 
    classpath "com.google.protobuf:protobuf-gradle-plugin:$Version" 
    } 
} 
    
protobuf { 
    protoc { 
        artifact = "com.google.protobuf:protoc:$Version" 
    }
    plugins { 
        grpc { 
            artifact = "io.grpc:protoc-gen-grpc-java:$Version" 
        } 
    } 
    generateProtoTasks { 
        all()*.plugins { 
            grpc {} 
        } 
    } 
}
```


### Implement & Override
```java
private static class RouteGuideService extends RouteGuideGrpc.RouteGuideImplBase {
  @Override
  public void getFeature(Point request, StreamObserver<Feature> responseObserver) {
    responseObserver.onNext(checkFeature(request));
    responseObserver.onCompleted();
  }
  
  private Feature checkFeature(Point location) {
    for (Feature feature : features) {
      if (feature.getLocation().getLatitude() == location.getLatitude()
          && feature.getLocation().getLongitude() == location.getLongitude()) {
        return feature;
      }
    }
  
    // No feature was found, return an unnamed feature.
    return Feature.newBuilder().setName("").setLocation(location).build();
  }
}
```


## More

### gRPC 와 Dagger 의 관계
[Dagger-gRPC](https://dagger.dev/dev-guide/grpc-servers.html) 를 쓰고있음
간단한 어노테이션(`@GrpcService`) 만으로 인스턴스를 DI 할 수 있다.

### gRPC 와 Web Application Server 의 관계
개념이 다름 JAVA 진영에서는 요청당 쓰레드(서블릿)를 생성하여 request 에 대한 대응을 하는데
이 서블릿의 컨테이너가 WAS(인터페이스)이고 톰캣은 이 WAS 의 구현체임

Spring MVC 기반으로 gRPC 서버를 구성하면 동기적으로 톰캣 위에서 통신,
Spring WebFlux 기반으로 구성하면 [netty](https://netty.io) 위에서 비동기적으로 통신할 수 있다.

netty 만으로 서버를 구축하고 Dagger-gRPC 와 연동하여 가볍게 gRPC 서버를 만들 수 있고,
[Armeria](https://armeria.dev), [vert.x](https://vertx.io) 와 같은 netty 기반 프레임워크를 통해 한 단계 윗레벨의 추상화도 가능함.