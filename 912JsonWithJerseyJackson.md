#	JSON example with Jersey + Jackson

-	Jersey uses Jackson to convert object to / form JSON.


## Steps to implement Json with Jersey Jax-rs

-	Add Jersey Json support dependency 
-	Add init-param of POJO Mapping Feature for Jersey Servlet in Web.xml file 
-	Put @Produces(MediaType.APPLICATION_JSON)

## 	Dependency

-	To make Jersey to support JSON mapping, declare “jersey-json.jar” in Maven pom.xml file.
	
	Dependency:

		<dependency>
			<groupId>com.sun.jersey</groupId>
			<artifactId>jersey-server</artifactId>
			<version>1.8</version>
		</dependency>

		<dependency>
			<groupId>com.sun.jersey</groupId>
			<artifactId>jersey-json</artifactId>
			<version>1.8</version>
		</dependency>	

## Integrate JSON with Jersey	

-	In web.xml, declare “com.sun.jersey.api.json.POJOMappingFeature” as “init-param” in Jersey mapped servlet.
- 	It will make Jersey support JSON/object mapping.

	<init-param>
		<param-name>com.sun.jersey.api.json.POJOMappingFeature</param-name>
		<param-value>true</param-value>
	</init-param>

##	web.xml – full example.

	Code Snippet:

		<web-app ...>

		  <servlet>
			<servlet-name>jersey-serlvet</servlet-name>
			<servlet-class>com.sun.jersey.spi.container.servlet.ServletContainer</servlet-class>
			<init-param>
				<param-name>com.sun.jersey.config.property.packages</param-name>
				<param-value>com.mkyong.rest</param-value>
			</init-param>
			<init-param>
				<param-name>com.sun.jersey.api.json.POJOMappingFeature</param-name>
				<param-value>true</param-value>
			</init-param>
			<load-on-startup>1</load-on-startup>
		  </servlet>

		  <servlet-mapping>
			<servlet-name>jersey-serlvet</servlet-name>
			<url-pattern>/rest/*</url-pattern>
		  </servlet-mapping>

		</web-app>


## 	Simple Object


-	A simple “Track” object, later Jersey will convert it into JSON format.

		public class Track {

			String title;
			String singer;

			public String getTitle() {
				return title;
			}

			public void setTitle(String title) {
				this.title = title;
			}

			public String getSinger() {
				return singer;
			}

			public void setSinger(String singer) {
				this.singer = singer;
			}

			@Override
			public String toString() {
				return "Track [title=" + title + ", singer=" + singer + "]";
			}

		}

##	JAX-RS with Jersey Implementation

-	Annotate the method with @Produces(MediaType.APPLICATION_JSON).
-	Jersey will use Jackson to handle the JSON conversion automatically.


	Code Snippet:

		import javax.ws.rs.Consumes;
		import javax.ws.rs.GET;
		import javax.ws.rs.POST;
		import javax.ws.rs.Path;
		import javax.ws.rs.Produces;
		import javax.ws.rs.core.MediaType;
		import javax.ws.rs.core.Response;

		import com.mkyong.Track;

		@Path("/json/metallica")
		public class JSONService {

			@GET
			@Path("/get")
			@Produces(MediaType.APPLICATION_JSON)
			public Track getTrackInJSON() {

				Track track = new Track();
				track.setTitle("Enter Sandman");
				track.setSinger("Metallica");

				return track;

			}

			@POST
			@Path("/post")
			@Consumes(MediaType.APPLICATION_JSON)
			public Response createTrackInJSON(Track track) {

				String result = "Track saved : " + track;
				return Response.status(201).entity(result).build();
				
			}
			
		}

##	Demo

-	1. GET method

	-	When URI pattern “/json/metallica/get” is requested, 
	-	he Metallica classic song “Enter Sandman” will be returned in JSON format.

##	2.	POST Method

	-	“post” the json format string to URI pattern “/json/metallica/post“, the posted json string will be converted into “Track” object automatically.

