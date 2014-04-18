##Documentation
If you want see in the repository, [**click here**](https://github.com/AttwFramework/Documentation).

###Tip
Study the MVC pattern before to use any MVC framework.

* [Configurations](https://github.com/AttwFramework/Documentation/blob/master/CONFIG.md)
* [Controller](https://github.com/AttwFramework/Documentation/blob/master/CONTROLLER.md)
* [Database Manager](https://github.com/AttwFramework/Documentation/blob/master/DATABASE.md)
* [Files](https://github.com/AttwFramework/Documentation/blob/master/FILES.md)
* [HTTP](https://github.com/AttwFramework/Documentation/blob/master/HTTP.md)
* [Logger](https://github.com/AttwFramework/Documentation/blob/master/LOGGER.md)
* [Model](https://github.com/AttwFramework/Documentation/blob/master/MODEL.md)
* [Paginator](https://github.com/AttwFramework/Documentation/blob/master/PAGINATOR.md)
* [Routing](https://github.com/AttwFramework/Documentation/blob/master/ROUTING.md)
* [View](https://github.com/AttwFramework/Documentation/blob/master/VIEW.md)

###Configurations
The configurations...
***
###Controller
To create an controller, create a class that extends ```Attw\Controller\AbstractController```.

####Methods

#####Default action
When a action don't be indcated, the default will be ```index```.

#####Load model
To load a model, use the ```Attw\Controller\AbstractController::loadModel( string $model )```.

#####Load view
To load a view, use the ```Attw\Controller\AbstractController::loadView( string $view )```.

####Controller example
```php
namespace MVC\Controller;

use Attw\Controller\AbstractController;

class HelloWorld extends AbstractController{
    public function index(){
        echo 'Hello, world!';
    }
}
```
***
###Database Manager
The database manager...
***
###Files
####File object
To use a file how object, use ```Attw\File\File```.

####Upload
To do a upload of a file, use ```Attw\File\Uploader\Upload```.

####Upload example
```php
namespace MVC\Controller;

use Attw\Controller\AbstractController;
use Attw\File\File;
use Attw\File\Uploader\Upload;
use Attw\HTTP\Request;

class SomeController extends AbstractController{
    public function index(){
        $request = new Request();
        $file = new File( $request->file( 'image' ) );
        $upload = new Upload();
        $upload->upload( $file, 'Application/Public/uploads' );
    }
}
```
***
###HTTP
Read and send HTTP requests and responses.

####Requests
Read and send HTTP requests with ```Attw\HTTP\Request```.
#####Send a request
Use ```Attw\HTTP\Request::send( Attw\HTTP\Request\RequestInterface $request )```.
#####Requests
Implementing ```Attw\HTTP\Request\RequestInterface```:
* QueryString (```$_GET```): ```Attw\HTTP\Request\Query```.
* Post (```$_POST```): ```Attw\HTTP\Request\Post```.

**Observation:** Needs [**cURL**](https://php.net/manual/pt_BR/book.curl.php) extension to use this resource.

#####Request class methods
* ```Attw\HTTP\Request::post( string $property )```: Read a post requested
* ```Attw\HTTP\Request::server( string $property )```: Read a server requested
* ```Attw\HTTP\Request::query( string $property )```: Read a querystring requested
* ```Attw\HTTP\Request::file( string $property )```: Read a file requested
* ```Attw\HTTP\Request::isPost()```: Verify if exists a post request
* ```Attw\HTTP\Request::isQuery()```: Verify if exists a querystring request
* ```Attw\HTTP\Request::isFiles()```: Verify if exists a file request
* ```Attw\HTTP\Request::isAjax()```: Verify is an Ajax request
* ```Attw\HTTP\Request::issetPost( string $property )```: Verify if exists a determined post request
* ```Attw\HTTP\Request::issetQuery( string $property )```: Verify if exists a determined querystring request
* ```Attw\HTTP\Request::issetFile( string $property )```: Verify if exists a determined file request
* ```Attw\HTTP\Request::issetServer( string $property )```: Verify if exists a determined server property
* ```Attw\HTTP\Request::getMethod()```: Return request method

#####Send Request example
```php
namespace MVC\Controller;

use Attw\Controller\AbstractController;
use Attw\HTTP\Request\Post;

class SomeController extends AbstractController{
    public function index(){
        $post = new Post( 'http://whatever.com', [ 'whatever-info' => 'whatever' ] );
        $post->send();

        /**
         * You can do this also:
         * $request = new Attw\HTTP\Request();
         * $post = new Post( 'http://whatever.com', [ 'whatever-info' => 'whatever' ] );
         * $request->send( $post );
        */
    }
}
```

####Response
Read and send HTTP responses with ```Attw\HTTP\Response```.

#####Response class methods
* ```Attw\HTTP\Response::setProtocol( string $protocol )```: Set the HTTP protocol (default: ```HTTP/1.1```)
* ```Attw\HTTP\Response::sendHeader( string $name, string $value = null )```: Send a header. If ```$value``` is null, header content is ```$value```
* ```Attw\HTTP\Response::setStatusCode( int $code )```: Set the HTTP status code
* ```Attw\HTTP\Response::getStatusCode()```: Return the HTTP status code
* ```Attw\HTTP\Response::addHeader( string $name, string $value = null )```: Add a header to send
* ```Attw\HTTP\Response::sendAllHeaders()```: Send all headers added to send
* ```Attw\HTTP\Response::redirect( string $url, int $statusCode = 200 )```: Redirect to an URL. ```$statusCode``` indicates the status code necessary to redirect
* ```Attw\HTTP\Response::senStatusCode( int $statusCode )```: Send a HTTP status code

#####Response example
```php
namespace MVC\Controller;

use Attw\Controller\AbstractController;
use Attw\HTTP\Response;

class SomeController extends AbstractController{
    public function index(){
        $response = new Response();
        $response->sendHeader( 'Content-type', 'text/html; Charset=UTF-8' );
    }
}
```

####Cookies
Read and manager cookies with ```Attw\HTTP\Cookie\Manager```.

#####Cookies manager class methods
* ```Attw\HTTP\Cookie\Manager::add( string $name [, mixed $value = null, int $expire = 0, string $path = '/', string $domain = null, boolean $secure = false, boolean $httponly = false ] )```: Create a cookie
* ```Attw\HTTP\Cookie\Manager::read( string $name )```: Read a cookie
* ```Attw\HTTP\Cookie\Manager::del( string $name )```: Delete a cookie
* ```Attw\HTTP\Cookie\Manager::delAll()```: Delete all cookies
* ```Attw\HTTP\Cookie\Manager::exists( string $name )```: Verify if a cookie exists

***
###Logger
A system to logs manager.

####Methods
The commom methods of a logger are:
* ```Attw\Logger\LoggerInterface::write( string | int $type, string $message )```: Write a log
* ```Attw\Logger\AbstractLogger::setLogStructure( string $logStructure )```: All parameters must initiate with ```:```. Default structure: ```[:type | :date] :message```.
* ```Attw\Logger\AbstractLogger::addCustomizedCamp( array | string $camp, string $value = null )```: Add a customized camp to log structure.

####File logger
The file logger will be registered logs in a file.

#####File logger methods
* ```Attw\Logger\Log\File::setFilet( string $file )```: indicates which file will be registered the logs

####Local of type logs
To configure the locals that will be saved, go to Configurations.php and, in Logs, create a index called ```LogTypesLocals```:
```php
	$logs = array(  /* Other configurations */
                /**
                  * Files of logs in general
                */
                'SystemLogs' => array( 
				    'File' => APP_ROOT . DS . 'Application' . DS . 'logs.txt' 
			    ),
                /**
                  * Files of logs by type
                */
		        'LogTypesLocals' => array( 
			           LOG_WARNING => APP_ROOT . DS . 'Application' . DS . 'logs' . DS . 'logs-warning.txt',
                       LOG_ALERT => APP_ROOT . DS . 'Application' . DS . 'logs' . DS . 'logs-alert.txt'
			)
	);
```

####Exmaple
```php
namespace MVC\Controller;

use Attw\Logger\Log\File;

class SomeController extends AbstractController{
    public function index(){
        echo 'Message!';

        File::setFile( 'logs.txt' );
        File::write( File::LOG_INFO, 'Message displayed successfully' );
    }
}
```

***
###Model
To create a model, create a class that extends ```Attw\Model\AbstractModel```.

####Entities
An entity will extend ```Attw\Model\Entity\AbstractEntity```.

#####Primary keys
Is necessary indicates what is the primary key of table. To do this, put that comment on the property: 
```php
/**
 * @key PRIMARY KEY
*/
```

#####Table
To indicates what is the table of entity, create a property called ```$table```.

#####Entity example
```php
namespace MVC\Model\Entity;

use Attw\Model\Entity\AbstractEntity;

class Messages extends AbstractEntity{
    protected $table = 'messages';
    
    /**
     * @key PRIMARY KEY
    */
    protected $id;

    protected $message;
}
```

####Properties

#####Storage
To use some storage resource, use the property ```Attw\Model\AbstractModel::$storage```.

#####Entity Storage
To use some entity storage resource, use the property ```Attw\Model\AbstractModel::$entity```.

####Model example
```php
namespace MVC\Model;

use Attw\Model\AbstractModel;

class Messages extends AbstractModel{
    public function save( $message ){
        $this->storage->insert( 'messages', [ 'message' => $message ] );
    }
}
```
***
###Paginator
Paginate contents with this.

####Methods
* ```Attw\Paginator\PaginatorInterface::getPaginated()```: Return an array or object with paginated content
* ```Attw\Paginator\PaginatorInterface::count()```: Counts how many pages the content was paginated
* ```Attw\Paginator\PaginatorInterface::getCurrentPage()```: Return the current page of pagination

####Array Paginator
To paginate an array, use ```Attw\Paginator\ArrayPaginator```.

#####Constrcutor structure
```Attw\Paginator\ArrayPaginator::__construct( array $data, int $limit, int $currentPage [, string $sort = null ] )```

```$sort```: Choose what organization type (descendant: ```Attw\Paginator\ArrayPaginator::SORT_DESC``` or ascendant: ```Attw\Paginator\ArrayPaginator::SORT_ASC```). Case ```null```, without organization.

####Paginator example
```php
namespace MVC\Controller;

use Attw\Controller\AbstractController;
use Attw\Paginator\ArrayPaginator;

class SomeController extends AbstractController{
    public function index(){
        $arr = [ 1, 2, 3, 4, 5, 6, 7, 8 ];
        $paginator = new ArrayPaginator( $arr, 2, 1, ArrayPaginator::SORT_ASC );
        print_r( $paginator->getPaginated() );
        echo 'Paginated in ' . $paginator->count() . ' pages';
    }
}
```
***
###Routing
The rounting...
***
###View
To create a view, create a class that extends ```Attw\View\AbstractView```.

####Templates
The templates are render with [**Smarty**](http://www.smarty.net/).

The template files must be saved with ```.tpl``` extension.

And to set the template variables, create a protected method called ```toRender``` in your view class.

####View example
Template file:
```html
<!doctype html>
<html>
    <head>
        <title>Message</title>
    </head>
    <body>
        <h1>{$message}</h1>
    </body>
</html>
```

View class:
```php
namespace MVC\View;

use Attw\View\AbstractView;

class Message extends AbstractView{
    protected function toRender(){
        $this->smarty->assign( 'message', $this->vars['message'] );
    }
}
```

And the controller:
```php
namespace MVC\Controller;

use Attw\Controller\AbstractController;

class Message extends AbstractController{
    public function index(){
        $view = $this->loadView( 'Message' );
        $view->setTplFile( 'message.tpl' );
        $view->message = 'Hello, World!';
        $view->render();
    }
}
```
