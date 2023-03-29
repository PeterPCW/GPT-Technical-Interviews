<details>
  <summary>Develop a Java Spring Boot microservice that consumes a third-party API (e.g., OpenWeatherMap), and exposes a simplified API to retrieve weather data for a given location. The service should use Feign to call the external API, and implement basic error handling and retries.</summary>
  
  Step 1: Create a Spring Boot Project
  To create a Spring Boot project, you can use the Spring Initializr (**[https://start.spring.io/](https://start.spring.io/)**) website. Select the necessary dependencies, such as Spring Web, Spring Cache, and Feign.

  Step 2: Define the API Contract
  Define the API contract in a Java interface using Spring Web annotations. For example:

  ```java
  public interface WeatherService {
      @GetMapping("/weather")
      WeatherData getWeather(@RequestParam String location);
  }
  ```

  Step 3: Implement the Service
  Implement the WeatherService interface using Feign to call the OpenWeatherMap API. For example:

  ```java
  @Service
  @CacheConfig(cacheNames = "weather")
  public class WeatherServiceImpl implements WeatherService {

      private final OpenWeatherMapClient openWeatherMapClient;

      public WeatherServiceImpl(OpenWeatherMapClient openWeatherMapClient) {
          this.openWeatherMapClient = openWeatherMapClient;
      }

      @Override
      @Cacheable(key = "#location")
      public WeatherData getWeather(String location) {
          return openWeatherMapClient.getWeatherData(location);
      }

      @CacheEvict(allEntries = true)
      public void evictAllCache() {
      }
  }
  ```

  In the implementation, we use **`@Cacheable`** to cache the result of **`getWeather()`** method for a given location. We also define a **`CacheEvict`** method to evict the cache when the underlying data changes.

  Step 4: Define the External API Client
  Define an external API client using Feign to call the OpenWeatherMap API. For example:

  ```java
  @FeignClient(name = "openweathermap", url = "${openweathermap.url}")
  public interface OpenWeatherMapClient {
      @GetMapping("/data/2.5/weather")
      WeatherData getWeatherData(@RequestParam String q);
  }
  ```

  Step 5: Configure the Application
  Configure the application by adding the necessary properties to **`application.properties`**. For example:

  ```java
  openweathermap.url=https://api.openweathermap.org
  openweathermap.appid=your_app_id
  ```

  Step 6: Test the Service
  Test the service by calling the API endpoint using curl or a web browser. For example:

  ```bash
  http://localhost:8080/weather?location=London,UK
  ```

  This will return weather data for London, UK.
</details>