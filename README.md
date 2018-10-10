Disconf 
=======
# 修改点
把ReloadingPropertyPlaceholderConfigurer类改为继承PropertySourcesPlaceholderConfigurer

# 注意事项
1. 不要再使用<context:property-placeholder>标签来引入本地配置文件
2. 不要再使用@PropertySource注解来引入本地配置文件
3. 不要再使用其他PropertyPlaceholderConfigurer来引入本地配置文件
4. 不要再使用其他PropertySourcesPlaceholderConfigurer来引入本地配置文件

# demo
'''Java

@Configuration
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class ApplicationConfig {
 
    @Bean(destroyMethod = "destroy")
    public BeanDefinitionRegistryPostProcessor disconfMgrBean() {
        return new DisconfMgrBean();
    }
 
    @Bean(initMethod = "init", destroyMethod = "destroy")
    public DisconfMgrBeanSecond disconfMgrBeanSecond() {
        return new DisconfMgrBeanSecond();
    }
 
    @Bean
    @Primary
    public PropertiesFactoryBean propertiesFactory() throws IOException {
        ReloadablePropertiesFactoryBean factory = new ReloadablePropertiesFactoryBean();
        /*
         * 本地文件路径("classpath:/test.properties")放前面，远程文件名放后；这样远程文件的内容会覆盖本地文件的内容。
         * 需要配置-Ddisconf.ignore=test.properties，以避免disconf尝试从远程下载test.properties文件
         */
        factory.setLocations(Arrays.asList("classpath:/test.properties", "db.properties", "dubbo.properties"));
        factory.setFileEncoding(StandardCharsets.UTF_8.name());
        return factory;
    }
 
    @Bean
    public static PropertySourcesPlaceholderConfigurer properties(PropertiesFactoryBean factory) throws IOException {
        ReloadingPropertyPlaceholderConfigurer ppc = new ReloadingPropertyPlaceholderConfigurer();
        // 如果需要远程内容覆盖本地application.properties里的内容，需设置localOverride为true
        // ppc.setLocalOverride(true);
        ppc.setIgnoreUnresolvablePlaceholders(true);
        ppc.setIgnoreResourceNotFound(true);
        ppc.setProperties(factory.getObject());
        return ppc;
    }
 
}

'''
