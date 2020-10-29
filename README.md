# estore
在线购物系统
一、功能需求说明
1. 登录
在login.jsp页面上用户可以输入用户名和密码进行登录，如果用户名和密码都正确，跳转到index.jsp；如果不正确，跳转到login.jsp页面继续登录。
代码步骤：
(1) dao层，映射文件实现映射接口CustomerDao的
Customer findByUsername(String username);
(2) service层，实现ICustomerService接口的
Customer findByUsername(String username, String password) throws Exception;
调用dao层方法，
(3) web层，编写一个LoginServlet；接收前台login.jsp的参数；
调用service层的方法；容器对象放入对应的参数，最后进行相应的页面跳转。

2. 注册
在register.jsp页面注册一个新用户，用户名作为以后登陆唯一标识。当注册
成功时，跳转到login.jsp页面；注册失败时，跳转至register.jsp继续注册。
代码步骤：
(1) dao层，实现void saveCustomer(Customer customer);
(2) service层，实现saveCustomer()，注意：用户名唯一判断。
调用dao层方法。
(3) web层，编写RegisterServlet，接收前台参数，调用service层方法，
容器对象放入相应的参数，页面跳转。

3. 修改用户信息
当用户登录后，index界面右上角部分显示个人信息，点击个人信息，进入userinfo.jsp，显示当前当前登录系统的个人详细信息。点击修改后，刷新当前页面，显示用户最后更新的信息。
代码步骤：
(1) dao层实现void updateCustomer(Customer customer);
(2) service层实现void updateCustomer(Customer customer);
调用dao层对应方法
(3) web层编写UpdateServlet，接收前台参数，调用service层方法，
容器对象放入相应的参数，页面跳转。

4. index.jsp页面动态加载（从数据库取值）
作为一个线上购物系统，不论用户是否是登录状态，主页都应该去数据库加
载动态数据：所有图书分类、所有图书。
代码步骤：
(1) dao层实现findAllCategories()和findAllBooks()
(2) service层实现findAllCategories()和findAllBooks()，调用dao层方法。
(3) web层编写一个Servlet，调用service层方法，容器对象放入相应的参数，页面跳转。注意：如果是这种写法，如果我们启动项目直接去访问index界面，还是取不到动态数据，必须要经过我们写的这个servlet，所以这样方式不太合理。
我们编写一个监听器，当项目启动时，用容器对象存储所有的分类和图书。

5. 购物车
当用户登录后，才有购物车功能，当用户登录系统后，应该从数据库中把当前用户的购物车查询出来。
代码步骤：
(1) dao层，实现findByCustomer()
(2) service层，实现findByCustomer()，调用dao层对应的方法
(3) web层，当用户登录系统后，放入购物车，所以对应的是LoginServlet中（也可以使用监听器，监听容器对象中用户是否存在），调用service层相应方法。
5.1 加入购物车
如果加入的是一本新书（即购物车中之前不存在的书籍），insert；如果不是一本新书，我们应当对数量num进行操作，而不应该insert。
代码步骤：
(1) dao层，实现saveShopCar()和updateShopCar()
(2) service层，判断当前要加入到购物车的书籍是否在购物车中，如果不在，调用dao层方法插入；如果在，调用dao层方法更新购物车。
(3) web层，编写一个AddShopCarServlet，接收前台参数，调用servcie层响应方法。

6. 订单
用户登录后，可查看我的订单，即购物记录。
订单和订单项的关系：如果我们去超市购物，结账小票可看作是一个订单，那么我们购买的每样商品可看作是一个订单项，一个订单是由多个订单项组成的。
6.1 查看订单
代码步骤：
(1) dao层实现findByCustomer();
(2) service层实现findByCustomer()，调用dao层方法
(3) web层，编写ShowOrdersServlet，调用servcie层方法，容器对象存值，页面跳转。
6.2 生成订单
当我们从购物车中结算时，会生成一个订单。
代码步骤：
(1) dao层实现saveOrder(),saveOrderLine(),clearShopCarByCustomer()
(2) servcie层，根据要结算购物车中的书籍及其数量，封装order和orderline对象，调用dao层相应方法。
注意：逻辑处理，生成订单，生成订单项，从购物车中移除相应的书籍。
(3) web层，编写SaveOrderServlet，接收前台参数，调用service层方法，页面跳转。

		  


