Disconf 
=======
# 修改点：
把ReloadingPropertyPlaceholderConfigurer类改为继承PropertySourcesPlaceholderConfigurer

# 注意事项：
1. 不要再使用<context:property-placeholder>标签来引入本地配置文件
2. 不要再使用@PropertySource注解来引入本地配置文件
3. 不要再使用其他PropertyPlaceholderConfigurer来引入本地配置文件
4. 不要再使用其他PropertySourcesPlaceholderConfigurer来引入本地配置文件
