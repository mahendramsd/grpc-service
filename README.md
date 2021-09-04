# Intergrate gPRC with Spring Boot.

Spring Boot & gRPC integration

# Getting started with gRPC

This sample for intergrate gRPC with Spring Boot and write a service using the artifact create by https://github.com/mahendramsd/grpc-wapper
Dependancies

following dependencies are require to intergrate gRPC with Spring boot

	 implementation 'org.springframework.boot:spring-boot-starter-web'
	 implementation 'io.github.lognet:grpc-spring-boot-starter:3.4.3'
	 compileOnly 'org.projectlombok:lombok'
	 developmentOnly 'org.springframework.boot:spring-boot-devtools'
	 annotationProcessor 'org.projectlombok:lombok'

	 // locally published artifact from the above sample project
	 compile group: 'com.ices.vac', name: 'vac-service', version: '0.0.100'

	 implementation 'io.grpc:grpc-netty-shaded:1.40.1'
	 implementation 'io.grpc:grpc-protobuf:1.40.1'

	repositories {
		mavenCentral()
		mavenLocal()

	}

Add mavenLocal() to use the locally published artifact.
# Using as gRPC server.

Need to implement services annotated with '@GRpcService' also need to allocate a grpc port run as a gRPC server

in the following sample service we created by extending the sample service we have introduced in the protobuf project 'GreetingServiceGrpc'

	@GRpcService
	public class VacServiceGrpcImpl extends GreetingServiceGrpc.GreetingServiceImplBase {

	    @Override
	    public void greet(GreetingRequest request, StreamObserver responseObserver) {
		GreetingResponse response = GreetingResponse.newBuilder()
			.setResult("Hello Janaka "+ request.getGreeting().getFirstName())
			.build();

		responseObserver.onNext(response);
		responseObserver.onCompleted();
	    }
	}

Annotating the service with @GRpcService tells to serve as a gRPC server using the port given in the application.properties. grpc.port=6565

OOK, we have successfuly integrate gRPC with Spring boot and expose a service '/greet', now the time to test the service.


