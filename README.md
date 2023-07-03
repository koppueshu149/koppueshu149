- 👋 Hi, I’m @koppueshu149
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
koppueshu149/koppueshu149 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.velocity.VelocityConfigurer;
import org.springframework.web.servlet.view.velocity.VelocityViewResolver;

@Configuration
public class GatewayConfig implements WebMvcConfigurer {

    @Bean
    public ViewResolver viewResolver() {
        VelocityViewResolver resolver = new VelocityViewResolver();
        resolver.setPrefix("/templates/");
        resolver.setSuffix(".vm");
        resolver.setCache(false);
        return resolver;
    }

    @Bean
    public VelocityConfigurer velocityConfigurer() {
        VelocityConfigurer configurer = new VelocityConfigurer();
        configurer.setResourceLoaderPath("classpath:/templates/");
        return configurer;
    }
}
