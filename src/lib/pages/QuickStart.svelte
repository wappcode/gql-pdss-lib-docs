<script>
	import CodeBlock from '$lib/components/CodeBlock.svelte';
	import CodeBlockEndChar from '$lib/components/CodeBlockEndChar.svelte';
	import CodeBlockOpenChar from '$lib/components/CodeBlockOpenChar.svelte';
	import PageContent from './PageContent.svelte';
</script>

<PageContent>
	<h1>GQLPDSS Quick Start</h1>

	<h3>Propósito</h3>
	<p>Construir una api graphql para un blog</p>
	<h3>Funcionalidad</h3>

	<ul>
		<li>CRUD Autores</li>
		<li>CRUD Post</li>
		<li>CRUD Comentarios</li>
	</ul>
	<h3>Requisitos</h3>
	<ul>
		<li>PHP versión 7.4 o superior</li>
		<li>PHP Composer</li>
		<li>Conexión a base de datos MySQL</li>
	</ul>

	<h2>Crear proyecto</h2>
	<code> composer create-project wappcode/gql-pdss-skeleton my-project </code>

	<h3>Inicializar y configurar la base de datos</h3>
	<p>Es necesario tener configurado MySQL y crear una base de datos para esta práctica</p>

	<p>Crear el archivo config/doctrine.local.php con el siguiente código</p>

	<CodeBlock>
		&lt;?php return [ &quot;driver&quot; =&gt; [ &apos;user&apos; =&gt; &apos;root&apos;,
		&apos;password&apos; =&gt; &apos;rootpassword&apos;, &apos;driver&apos; =&gt;
		&apos;pdo_mysql&apos;, &apos;host&apos; =&gt; &apos;localhost&apos;, &apos;dbname&apos; =&gt;
		&apos;&apos;, &apos;charset&apos; =&gt; &apos;utf8mb4&apos; ], &quot;entities&quot; =&gt;
		require __DIR__ . &quot;/doctrine.entities.php&quot; ];
	</CodeBlock>

	<p>Reemplazar los datos de user, password y host conforme al servidor de la base de datos.</p>
	<p>Si se utiliza una base de datos diferente a mysql ajustar también el valor de driver</p>

	<p>
		Habilitar los permisos de escritura para la carpeta "data"<br />
		Linux:
		<code><pre>chmod a+w data</pre></code>
	</p>
	<p>
		<strong>Crear Entidad Author (Autor)</strong><br />
		Crear archivo modules/AppModule/src/Entities/Author con el siguiente código /****
	</p>

	<CodeBlock>
		&lt;?php namespace AppModule\Entities; use Doctrine\ORM\Mapping as ORM; use
		GPDCore\Entities\AbstractEntityModel; class Author extends AbstractEntityModel<CodeBlockOpenChar
		/>

		/** * * @ORM\Column(type=&quot;string&quot;, length=255, nullable=false,
		name=&quot;first_name&quot;) * @var string */ private $firstName; /** *
		@ORM\Column(type=&quot;string&quot;, length=255, nullable=true, name=&quot;last_name&quot;) * *
		@var ?string */ private $lastName; /** * @ORM\Column(type=&quot;string&quot;, length=255,
		nullable=false, name=&quot;email&quot;) * * @var string */ private $email; /** * Get the value
		of firstName * * @return string */ public function getFirstName()
		<CodeBlockOpenChar />
		return $this-&gt;firstName;
		<CodeBlockEndChar />

		/** * Set the value of firstName * * @param string $firstName * * @return self */ public
		function setFirstName(string $firstName)
		<CodeBlockOpenChar />
		$this-&gt;firstName = $firstName; return $this;
		<CodeBlockEndChar />

		/** * Get the value of lastName * * @return ?string */ public function getLastName()
		<CodeBlockOpenChar />
		return $this-&gt;lastName;
		<CodeBlockEndChar />

		/** * Set the value of lastName * * @param ?string $lastName * * @return self */ public function
		setLastName(?string $lastName)
		<CodeBlockOpenChar />
		$this-&gt;lastName = $lastName; return $this;
		<CodeBlockEndChar />

		/** * Get the value of email * * @return string */ public function getEmail()
		<CodeBlockOpenChar />
		return $this-&gt;email;
		<CodeBlockEndChar />

		/** * Set the value of email * * @param string $email * * @return self */ public function
		setEmail(string $email)
		<CodeBlockOpenChar />
		$this-&gt;email = $email; return $this;
		<CodeBlockEndChar />

		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		La clase Author es una entidad doctrine que utiliza anotaciones para la definición de sus
		columnas
	</p>

	<p>
		Esta clase extiende a la clase AbstractEntityModel que tiene como propósito agregar las
		propiedades id (Primary key), created y updated.
	</p>

	<p>
		Los métodos get son utilizados para generar los campos del tipo objeto graphql , para esto
		funcione es necesario que los métodos sean públicos y se les asigne un tipo de retorno ya sea
		por comentarios por ejemplo @return string o por la definición del tipo en el método por ejemplo
		public function getFirstName():string <CodeBlockOpenChar />
	</p>
	<p>
		Los métodos set son utilizados para genera los tipos inputs de graphql de la misma forma que los
		métodos get es necesario que sean públicos y asignarles e tipo requerido mediante comentarios
		(@param string $firstName) o en la definición del método ( public function setFirstName(string
		$firstName)<CodeBlockOpenChar /><CodeBlockEndChar />)
	</p>
	<p>
		Para generar el código sql que se debe ejecutar para actualizar la base de datos ejecutar el
		siguiente comando: vendor/bin/doctrine orm:schema-tool:update --dump-sql Para guardar los
		cambios en la base de datos ejecutar el comando vendor/bin/doctrine orm:schema-tool:update
		–forcerce
	</p>
	<h3>Registrar un campo graphql para crear autores.</h3>

	<p>
		En el archivo modules/AppModule/src/AppModule.php actuallizar el método getMutationFields para
		agregar la entrada 'createAuthor' de la siguiente forma.
	</p>
	<CodeBlock>
		function getMutationFields(): array
		<CodeBlockOpenChar />; return [ &apos;createAuthor&apos; =&gt;
		GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class) ];
		<CodeBlockEndChar />;
	</CodeBlock>
	<p>
		En el array que retorna el método getMutationsFields se agrega el campo graphql en donde la
		clave será el nombre del campo para hacer la consulta graphql y el valor es un objeto graphql
		creado con la clase GPDFieldFactory y el método buildFieldCreate
	</p>
	<p>No olvidar importar las clase GPDFieldFactory utilizando namespace</p>

	<CodeBlock>use GPDCore\Graphql\GPDFieldFactory; use AppModule\Entities\Author;</CodeBlock>
	<h3>Probar crear un autor.</h3>
	<p>
		La url de la api depende de la configuración del servidor y tiene como entrada principal el
		archivo public/index.php/api
	</p>
	<p>Por ejemplo en un servidor apache : http://localhost/my-project/public/index.php/api</p>

	<p>
		También se podría iniciar un servidor php desde el directorio de la aplicación con el siguiente
		comando
	</p>
	<CodeBlock>php -S localhost:8000 public/index.php</CodeBlock>
	<p>De esta forma la url sería http://localhost:8080/api</p>

	<p>
		Para realizar una prueba se puede utilizar una extensión del navegador para grapql o Postman.
	</p>
	<p>La consulta a realizar es la siguiente</p>

	<CodeBlock>
		mutation <CodeBlockOpenChar />
		createAuthor(input: <CodeBlockOpenChar />
		firstName: &quot;John&quot;, lastName: &quot;Doe&quot;, email: &quot;demo@demo.local.lan&quot;
		<CodeBlockEndChar />)<CodeBlockOpenChar />
		id firstName lastName email created updated

		<CodeBlockEndChar />
		<CodeBlockEndChar />
	</CodeBlock>
	<p>Con la extensión GraphQL Playground para Chrome podemos ejecutar la consulta.</p>
	<p>
		<img src="/images/create-author.png" alt="" />
	</p>
	<p>
		Al ejecutar la consulta se va a guardar un registro en la base de datos, los campos id, created
		y updated son creados atomáticamente.
	</p>
	<h3>Registrar un campo graphql para recuperar el registro de un autor por id.</h3>
	<p>
		En el archivo modules/AppModule/src/AppModule.php actuallizar el método getQueryFields para
		agregar la entrada 'getAuthor' de la siguiente forma.
	</p>
	<CodeBlock>
		function getQueryFields(): array
	<CodeBlockOpenChar />
	       return [ &apos;echo&apos; =&gt; [ &apos;type&apos; =&gt; Type::nonNull(Type::string()), &apos;args&apos;
	      =&gt; [ &apos;message&apos; =&gt; Type::nonNull(Type::string()) ], &apos;resolve&apos; =&gt; function
	      ($root, $args) <CodeBlockOpenChar />
	      return $args[&quot;message&quot;];
	     <CodeBlockEndChar />
	       ], &apos;getAuthor&apos;=&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class),
	    ];
	<CodeBlockEndChar />
</CodeBlock>
<p>De la misma forma que createAuthor en el array que retorna el método getQueryFields se agrega el campo graphql en donde la clave será el nombre del campo para hacer la consulta graphql y el valor es un objeto graphql creado con la clase GPDFieldFactory y el método buildFieldItem</p>
<p>Para probar obtene getAuthor se ejecuta el siguiente query en la extensión de Chrome graphql playground</p>

<CodeBlock>
    query <CodeBlockOpenChar />
        getAuthor(id:&quot;1&quot;)<CodeBlockOpenChar />
          id
          firstName
          lastName
          email
          created
          updated
        <CodeBlockEndChar />
        
      <CodeBlockEndChar />
   
      
</CodeBlock>
<p>
    <img src="/images/get-author.png" alt="">
  </p>
  <p>
    Para la consulta del autor puede ser conveniente agregar el nombre completo (fullName), para esto primero registramos un método getFullName en la clase Author
  </p>
  <CodeBlock>
    /**
    * Gets the value de fullname
    *
    * @return string
    */
   public function getFullName()<CodeBlockOpenChar />
       return $this-&gt;firstName .&quot; &quot;.$this-&gt;lastName ?? &quot;&quot;;
<CodeBlockEndChar />
</CodeBlock>
<p>Con esto podemos recuperar el nombre completo de un autor desde la clase Author en php y queda registrado la propiedad fullName en el esquema de grapqhl.</p>
<p><img src="/images/get-author-fullname-schema.png" alt=""></p>
<p>
    Sin embargo, como fullName no es una propiedad relacionada a una columna de la clase Author graphql aún no sabe con que dato se le debe asignar, para que esto función es necesario agregar un resolver de la clase Author para fullName
</p>
<p>
    En el archivo modules/AppModule/src/AppModule.php actualizar el método getResolvers de la siguiente forma.
</p>
<CodeBlock>
    function getResolvers(): array
    <CodeBlockOpenChar />
        return [
            &apos;Author::fullName&apos;=&gt; function($root, $args, $context, $info)<CodeBlockOpenChar />
                return $root[&quot;firstName&quot;].&quot; &quot;.$root[&quot;lastName&quot;] ??&apos;&apos;;

            <CodeBlockEndChar />
        ];
    <CodeBlockEndChar />

</CodeBlock>

<p>
    Aquí en el array que retorna el método getResolvers se agrega una función que se encargará de retornar el dato correcto, en este caso para fullName de Author.
</p>
<p>Es importante que la clave del array este compuesta por el nombre de la clase seguida de dos veces dos puntos ( :: ) seguido del nombre de la propiedad en este caso Author::fullName</p>
<p>El primer valor que se pasa a la función ($root) es un array que tiene todos los datos de la consulta principal en este caso todas las propiedades de la clase Author que corresponden a una columna de la base de datos.</p>
<p>Ahora se puede realizar la consulta solicitando el valor de fullName</p>
<p><img src="/images/get-author-fullname-schema.png" alt=""></p>
<h3>Registrar un campo graphql para recuperar la lista de los autores registrados.</h3>
<p>En el archivo modules/AppModule/src/AppModule.php  actuallizar el método getQueryFields para agregar la entrada 'getAuthorsList' de la siguiente forma.</p>

<CodeBlock>
    function getQueryFields(): array
    <CodeBlockOpenChar />
        return [
            &apos;echo&apos; =&gt;  [
                &apos;type&apos; =&gt; Type::nonNull(Type::string()),
                &apos;args&apos; =&gt; [
                    &apos;message&apos; =&gt; Type::nonNull(Type::string())
                ],

                &apos;resolve&apos; =&gt; function ($root, $args) <CodeBlockOpenChar />
                    return $args[&quot;message&quot;];
                <CodeBlockEndChar />
            ],
            &apos;getAuthor&apos;=&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class),
            &apos;getAuthorsList&apos;=&gt; GPDFieldFactory::buildFieldList($this-&gt;context, Author::class),
        ];
    <CodeBlockEndChar />

</CodeBlock>
<p>Ahora se puede hacer una consulta graphql para recuperar todos los autores.</p>
<p>Antes de hacer la prueba podemos crear más autores con la entrada createAuthor para poder visualizar mejor el resultado.</p>
<p>Para probar se ejecuta el siguiente query en la extensión graqphql</p>

<CodeBlock>
    query <CodeBlockOpenChar />
        getAuthorsList<CodeBlockOpenChar />
          fullName
          firstName
          lastName
          id
          created
          updated
          
          
        <CodeBlockEndChar />
      <CodeBlockEndChar />
      
</CodeBlock>
<p><img src="/images/authors-list.png" alt=""></p>
<p>
    La consulta getAuthorList ya incluye opciones para ordenar y filtrar, por ejemplo para filtrar los resultados por nombre se puede ejecutar la siguiente consulta.
</p>

<CodeBlock>

    query <CodeBlockOpenChar />
        getAuthorsList(filter:[
          <CodeBlockOpenChar />
            conditions:[
              <CodeBlockOpenChar />
                filterOperator:LIKE
                value:<CodeBlockOpenChar />
                  single: "%john%"
                <CodeBlockEndChar />,
                property: "firstName",
                
              <CodeBlockEndChar />
            ]
          <CodeBlockEndChar />
        ])<CodeBlockOpenChar />
          fullName
          firstName
          lastName
          id
          created
          updated
          
          
        <CodeBlockEndChar />
      <CodeBlockEndChar />
      
</CodeBlock>
<p>
    <img src="/images/authors-list-filter.png" alt="">
</p>
<p>Y para ordenar por nombre como ejemplo se puede ejecutar el siguiente query</p>
<CodeBlock>
    query <CodeBlockOpenChar />
        getAuthorsList(sorting:[
          <CodeBlockOpenChar />
            property:"firstName",
            direction:desc
          <CodeBlockEndChar />
        ])<CodeBlockOpenChar />
          fullName
          firstName
          lastName
          id
          created
          updated
          
          
        <CodeBlockEndChar />
      <CodeBlockEndChar />
      
</CodeBlock>
<p>NOTA: Se pueden utilizar los filtros y el orden  en la misma consulta</p>
<h3>Registrar un campo graphql para recuperar la lista paginada (graphql connection) de los autores  registrados.</h3>
<p>
    Antes de agregar la entrada para consultar la lista paginada es necesario registrar los tipos connection y Edge para el tipo de dato que se va a consultar, en este caso Author
</p>
<p>
    En la carpeta modules/AppModule/src/Graphql crear el archivo AuthorEdgeFactory.php con el siguiente código
</p>
<CodeBlock>
    &lt;?php

namespace AppModule\Graphql;

use GPDCore\Library\AbstractEdgeTypeServiceFactory;

class AuthorEdgeFactory extends AbstractEdgeTypeServiceFactory <CodeBlockOpenChar />
    
    const NAME = &apos;AuthorEdge&apos;;
    const DESCRIPTION = &apos;&apos;;
    protected static $instance = null;
<CodeBlockEndChar />

</CodeBlock>
<p>Esta clase extiende a AbstractEdgeTypeServiceFactory que tiene los métodos necesarios para construir el objeto grapqhl AuthorEdge</p>
<p>La constante NAME corresponde al nombre del objeto graphql que se va a crear y no debe caracteres especiales ni espacios en blanco.</p>
<p>La constante DESCRIPTION corresponde a la descripción del objeto graphql que se va a crear</p>
<p>La propiedad estática $instance es importante porque es donde se almacenará la instancia del objeto graphql una vez que se haya creado, de esta forma solo se creará un solo objeto de este tipo las siguientes solicitudes para obtener el objeto retornarán el objeto creado previamente.</p>
<p>Todos los objetos graphql deben ser registrados como servicios, entonces para crear y registrar como servicio el objeto AuthorEdge se actualiza el método getServicesAndGQLTypes de la clase AppModule en el archivo modules/AppModule/src/AppModule.php de la siguiente forma</p>
<CodeBlock>
    function getServicesAndGQLTypes(): array
    <CodeBlockOpenChar />
        return [
            &apos;invokables&apos; =&gt; [],
            &apos;factories&apos; =&gt; [
                AuthorEdgeFactory::NAME =&gt; AuthorEdgeFactory::getFactory($this-&gt;context,Author::class)
            ],
            &apos;aliases&apos; =&gt; []
        ];
    <CodeBlockEndChar />

</CodeBlock>
<p>
    En el array factories se agrega la clave con el nombre del nuevo objeto (AuthorEdgeFactory::NAME) y como valor se agrega el retorno del m&eacute;todo AuthorEdgeFactory::getFactory con los par&aacute;metros contexto ($this-&gt;context) y La clase de la entidad relacionada (Auth::class)
</p>
<p>Para el objeto AuthorConnection se realiza un proceso similar:</p>
<p>En la carpeta modules/AppModule/src/Graphql se crea el archivo AuthorConnectionFactory.php con el siguiente código.</p>


<CodeBlock>
    &lt;?php

namespace AppModule\Graphql;

use GPDCore\Library\AbstractConnectionTypeServiceFactory;

class AuthorConnectionFactory extends AbstractConnectionTypeServiceFactory <CodeBlockOpenChar />
    const NAME = &apos;AuthorConnection&apos;;
    const DESCRIPTION = &apos;&apos;;
    protected static $instance = null;

<CodeBlockEndChar />

</CodeBlock>

<p>NAME, DESCRIPTION e $instance tienen la misma funcionalidad que en AuthorEdgeFactory</p>
<p>Se agrega como servicio en la clase AppModule:</p>
<CodeBlock>

    function getServicesAndGQLTypes(): array
    <CodeBlockOpenChar />
        return [
            &apos;invokables&apos; =&gt; [],
            &apos;factories&apos; =&gt; [
                AuthorEdgeFactory::NAME =&gt; AuthorEdgeFactory::getFactory($this-&gt;context,Author::class),
                AuthorConnectionFactory::NAME =&gt; AuthorConnectionFactory::getFactory($this-&gt;context, AuthorEdgeFactory::NAME)
            ],
 
        ];
    <CodeBlockEndChar />

</CodeBlock>
<p>Para este caso a AuthorConnectionFactory::getFactory se le pasan las propiedades context y el nombre del objeto Edge relacionado en este caso AuthorEdgeFactory::NAME ("AuthorEdge")</p>
<p>Siempre es importante recordar que para importar otras clases es necesario agregar los namespaces al inicio del archivo  pro ejemplo en el archivo AppModule  después de la línea</p>

<CodeBlock>
    namespace AppModule;
</CodeBlock>
<p> se agregan las líneas</p>
<CodeBlock>
    use AppModule\Graphql\AuthorConnectionFactory;
    use AppModule\Graphql\AuthorEdgeFactory;

</CodeBlock>
<p>Una vez creados los tipos AuthorEdge y AuthorConnection se puede actualizar el método getQueryFields de la clase AppModule para agregar la consulta paginada</p>

<CodeBlock>

    function getQueryFields(): array
    <CodeBlockOpenChar />

        $authorConnection = $this-&gt;context-&gt;getServiceManager()-&gt;get(AuthorConnectionFactory::NAME);
        return [
            &apos;echo&apos; =&gt;  [
                &apos;type&apos; =&gt; Type::nonNull(Type::string()),
                &apos;args&apos; =&gt; [
                    &apos;message&apos; =&gt; Type::nonNull(Type::string())
                ],

                &apos;resolve&apos; =&gt; function ($root, $args) <CodeBlockOpenChar />
                    return $args[&quot;message&quot;];
                <CodeBlockEndChar />
            ],
            &apos;getAuthor&apos;=&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class),
            &apos;getAuthorsList&apos;=&gt; GPDFieldFactory::buildFieldList($this-&gt;context, Author::class),
            &apos;getAuthorsPaginatedList&apos;=&gt; GPDFieldFactory::buildFieldConnection($this-&gt;context,$authorConnection,  Author::class),
        ];
    <CodeBlockEndChar />

</CodeBlock>
<p>El código        </p>
<CodeBlock>
    $authorConnection = $this-&gt;context-&gt;getServiceManager()-&gt;get(AuthorConnectionFactory::NAME);
</CodeBlock>
<p>Recupera el objeto grapql AutorConnection desde el administrador de servicios y la línea</p>
<CodeBlock>
    &apos;getAuthorsPaginatedList&apos;=&gt; GPDFieldFactory::buildFieldConnection($this-&gt;context,$authorConnection,  Author::class),
</CodeBlock>
<p>Es donde se agrega la entrada para consultar autores de forma paginada</p>
<p>Para probarlo se puede ejecutar el siguiente query</p>
<p><img src="/images/authors-list-pagination.png" alt=""></p>
<p>En este caso se utiliza el input, para recuperar las primeras n registros</p>
<CodeBlock>
    <CodeBlockOpenChar />
        pagination:<CodeBlockOpenChar />
          first:1
        <CodeBlockEndChar />
      <CodeBlockEndChar />
    
      <p>
        El resultado contiene la siguiente información
      </p>
      <ul>
        totalCount: total de elementos.
pageInfo: Información de la página
edges: En la propiedad nodes se encuentran los datos de los registros consultados

      </ul>
</CodeBlock>
<p>
    El resultado contiene la siguiente información
</p>
<ul>
    <li>totalCount: total de elementos.</li>
    <li>pageInfo: Información de la página</li>
    <li>edges: En la propiedad nodes se encuentran los datos de los registros consultados</li>

</ul>
<p>Al igual que la consulta de listas se tiene la opción para filtrar y ordenar los elementos.</p>

<h3>Registrar un campo graphql para actualizar autores.</h3>
<p>En el archivo modules/AppModule/src/AppModule.php  actuallizar el método getMutationFields para agregar la entrada 'updateAuthor' de la siguiente forma.</p>

<CodeBlock>
    function getMutationFields(): array
    <CodeBlockOpenChar />
        return [
            &apos;createAuthor&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class),
            &apos;updateAuthor&apos; =&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Author::class),
        ];
    <CodeBlockEndChar />

</CodeBlock>
<p>Probar funcionamiento: En la extensión del navegador para graphql ejecutar la siguiente consulta</p>

<CodeBlock>
    mutation UpdateAuthor<CodeBlockOpenChar />
        updateAuthor(id:&quot;1&quot;, input:<CodeBlockOpenChar />
          email: &quot;Email Actualizado&quot;
        <CodeBlockEndChar />)<CodeBlockOpenChar />
          id
          fullName
          email
        <CodeBlockEndChar />
      <CodeBlockEndChar />
      
</CodeBlock>
<p>Para las actualizaciones los inputs son parciales lo que significa que se puede actualizar ya sea todos o solo algunos de datos del registro</p>
<h3>Registrar un campo graphql para eliminar autores.</h3>
<p>En el archivo modules/AppModule/src/AppModule.php  actuallizar el método getMutationFields para agregar la entrada deleteAuthorde la siguiente forma.</p>

<CodeBlock>
    function getMutationFields(): array
    <CodeBlockOpenChar />
        return [
            &apos;createAuthor&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class),
            &apos;updateAuthor&apos; =&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Author::class),
            &apos;deleteAuthor&apos; =&gt; GPDFieldFactory::buildFieldDelete($this-&gt;context, Author::class)
        ];
    <CodeBlockEndChar />

</CodeBlock>
<p>Probar funcionamiento: En la extensión del navegador para graphql ejecutar el siguiente código</p>
<CodeBlock>
    mutation DeleteAuthor<CodeBlockOpenChar/>
        deleteAuthor(id:&quot;1&quot;)
      <CodeBlockEndChar />
      
</CodeBlock>
<p>Como resultado la consulta retorna un valor boolean (falso o verdadero) indicando si se realizó correctamente la operación.</p>
<h3>
    Crear registro para POST
</h3>
<p>Crear entity</p>
<CodeBlock>

    &lt;?php
// modules/AppModule/src/Entities/Post.php
namespace AppModule\Entities;

use Doctrine\ORM\Mapping as ORM;
use AppModule\Entities\Author;
use GPDCore\Entities\AbstractEntityModel;

/**
 * @ORM\Entity
 * @ORM\Table(name=&quot;posts&quot;)
 */
class Post extends AbstractEntityModel
<CodeBlockOpenChar />
/**
     *
     * @ORM\Column(type=&quot;string&quot;, length=255, nullable=false, name=&quot;title&quot;)
     * @var string
     */
    private $title;
    /**
     * @ORM\Column(type=&quot;text&quot;, nullable=false, name=&quot;body&quot;)
     * @var string
     */
    private $body;    /**
     * @ORM\ManyToOne(targetEntity=&quot;\AppModule\Entities\Author&quot;, inversedBy=&quot;posts&quot;)
     * @ORM\JoinColumn(name=&quot;author_id&quot;, referencedColumnName=&quot;id&quot;, nullable=false)
     * @var Author
     */
    private $author;


    /**
     * Get the value of title
     *
     * @return  string
     */
    public function getTitle()
    <CodeBlockOpenChar />
        return $this-&gt;title;
<CodeBlockEndChar />

    /**
     * Set the value of title
     *
     * @param  string  $title
     *
     * @return  self
     */
    public function setTitle(string $title)
    <CodeBlockOpenChar />
        $this-&gt;title = $title;

        return $this;
<CodeBlockEndChar />

    /**
     * Get the value of body
     *
     * @return  string
     */
    public function getBody()
    <CodeBlockOpenChar />
        return $this-&gt;body;
<CodeBlockEndChar />

    /**
     * Set the value of body
     *
     * @param  string  $body
     *
     * @return  self
     */
    public function setBody(string $body)
    <CodeBlockOpenChar />
        $this-&gt;body = $body;

        return $this;
<CodeBlockEndChar />

    /**
     * Get the value of author
     *
     * @return  Author
     */
    public function getAuthor()
    <CodeBlockOpenChar />
        return $this-&gt;author;
<CodeBlockEndChar />

    /**
     * Set the value of author
     *
     * @param  Author  $author
     *
     * @return  self
     */
    public function setAuthor(Author $author)
    <CodeBlockOpenChar />
        $this-&gt;author = $author;

        return $this;
<CodeBlockEndChar />


</CodeBlock>
<h3>Se agrega a la clase Author la referencia a los posts </h3>
<CodeBlock>
    /**
     * 
     * @ORM\OneToMany(targetEntity=&quot;\AppModule\Entities\Post&quot;, mappedBy=&quot;author&quot;)
     * @var Collection
     */
    private $posts;

/**
     * Get the value of posts
     *
     * @return  Collection
     */
    public function getPosts(): Collection
    <CodeBlockOpenChar />
        return $this-&gt;posts;
    <CodeBlockEndChar />

    /**
     * Set the value of posts
     * @API\Exclude
     * @param  Collection  $posts
     *
     * @return  self
     */
    public function setPosts(Collection $posts)
    <CodeBlockOpenChar />
        $this-&gt;posts = $posts;

        return $this;
    <CodeBlockEndChar />
</CodeBlock>
<p>
    Como no vamos a crear post desde el registro de autores se agrega a la función setPost la anotación @API\Exclude para que ese método no sea considerado para definir el input de la clase Author.
</p>
<p>
    Es importante agregar use GraphQL\Doctrine\Annotation as API;  en la parte superior de la página para que se incluyan los archivos correspondientes 
</p>

<h3>Se ejecuta el comando para actualizar la base de datos</h3>
<CodeBlock>
    vendor/bin/doctrine orm:schema-tool:update --force
</CodeBlock>
<h3>De la misma forma que Author se agregan en AppModule las entradas para CRUD para  post</h3>

<p>Crear clase Edge para post</p>

<CodeBlock>
    &lt;?php

namespace AppModule\Graphql;
use GPDCore\Library\AbstractEdgeTypeServiceFactory;

class PostEdgeFactory extends AbstractEdgeTypeServiceFactory
<CodeBlockOpenChar />

 const NAME = &apos;PostEdge&apos;;
 const DESCRIPTION = &apos;&apos;  protected static $instance = null;
<CodeBlockEndChar />
</CodeBlock>
<p>Crear clase Connection para post</p>
<CodeBlock>
&lt;?php

namespace AppModule\Graphql;

use GPDCore\Library\AbstractConnectionTypeServiceFactory;

class PostConnectionFactory extends AbstractConnectionTypeServiceFactory
&lt;CodeBlockOpenChar /&gt;
    const NAME = &apos;PostConnection&apos;;
    const DESCRIPTION = &apos;&apos;;
    protected static $instance = null;
&lt;CodeBlockEndChar /&gt;
Registrar los tipos en ServiceManager en el m&eacute;todo  getServicesAndGQLTypes  de la clase AppModule
function getServicesAndGQLTypes(): array
    &lt;CodeBlockOpenChar /&gt;
        return [
            &apos;invokables&apos; =&gt; [],
            &apos;factories&apos; =&gt; [
                AuthorEdgeFactory::NAME =&gt; AuthorEdgeFactory::getFactory($this-&gt;context, Author::class),
                AuthorConnectionFactory::NAME =&gt; AuthorConnectionFactory::getFactory($this-&gt;context, AuthorEdgeFactory::NAME),
                PostEdgeFactory::NAME =&gt; PostEdgeFactory::getFactory($this-&gt;context, Post::class),
                PostConnectionFactory::NAME =&gt; PostConnectionFactory::getFactory($this-&gt;context, PostEdgeFactory::NAME)
            ],

        ];
    <CodeBlockEndChar />

</CodeBlock>
<p>Importarlos con</p>
<CodeBlock>
    use AppModule\Graphql\PostConnectionFactory;
use AppModule\Graphql\PostEdgeFactory;

</CodeBlock>

<p>Agregar las entradas graphql para ver los registros en el método getQueryFields de la clase AppModule</p>
<CodeBlock>
    function getQueryFields(): array
    <CodeBlockOpenChar />

        $authorConnection = $this-&gt;context-&gt;getServiceManager()-&gt;get(AuthorConnectionFactory::NAME);
        $postConnection = $this-&gt;context-&gt;getServiceManager()-&gt;get(PostConnectionFactory::NAME);
        return [
            &apos;echo&apos; =&gt;  [
                &apos;type&apos; =&gt; Type::nonNull(Type::string()),
                &apos;args&apos; =&gt; [
                    &apos;message&apos; =&gt; Type::nonNull(Type::string())
                ],

                &apos;resolve&apos; =&gt; function ($root, $args) <CodeBlockOpenChar />
                    return $args[&quot;message&quot;];
                <CodeBlockEndChar />
            ],
            &apos;getAuthor&apos; =&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class),
            &apos;getAuthorsList&apos; =&gt; GPDFieldFactory::buildFieldList($this-&gt;context, Author::class),
            &apos;getAuthorsPaginatedList&apos; =&gt; GPDFieldFactory::buildFieldConnection($this-&gt;context, $authorConnection,  Author::class),
            &apos;getPost&apos; =&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Post::class),
            &apos;getPostsList&apos; =&gt; GPDFieldFactory::buildFieldList($this-&gt;context, Post::class),
            &apos;getPostsPaginatedList&apos; =&gt; GPDFieldFactory::buildFieldConnection($this-&gt;context, $postConnection,  Post::class),
        ];
    <CodeBlockEndChar />

</CodeBlock>

<p>
    Agregar las entradas graphql para crear, actualizar y eliminar los registros en el método getMutationFields de la clase AppModule
</p>
<CodeBlock>
    function getMutationFields(): array
    <CodeBlockOpenChar />
        return [
            &apos;createAuthor&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class),
            &apos;updateAuthor&apos; =&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Author::class),
            &apos;deleteAuthor&apos; =&gt; GPDFieldFactory::buildFieldDelete($this-&gt;context, Author::class),
            &apos;createPost&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Post::class),
            &apos;updatePost&apos; =&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Post::class),
            &apos;deletePost&apos; =&gt; GPDFieldFactory::buildFieldDelete($this-&gt;context, Post::class)
        ];
    <CodeBlockEndChar />

</CodeBlock>

<h3>Crear resolver para obtener los datos del autor desde el post</h3>

<p>Se crea la clase PostResolversFactory en la carpeta modules/AppModule/src/Graphql</p>
<CodeBlock>
    &lt;?php

namespace AppModule\Graphql;

use AppModule\Entities\Author;
use GPDCore\Library\EntityBuffer;
use GPDCore\Library\ResolverFactory;

class PostResolversFactory
<CodeBlockOpenChar />

    public static function getAuthorResolver()
    <CodeBlockOpenChar />
        $entityBuffer = new EntityBuffer(Author::class);
        $resolver = ResolverFactory::createEntityResolver($entityBuffer, &apos;author&apos;);
        return $resolver;
    <CodeBlockEndChar />
<CodeBlockEndChar />

</CodeBlock>
<p>El método ResolverFactory::createEntityResolver recibe dos parámetros el primero es un objeto tipo EntityBuffer que se encarga de optimizar la consulta para obtener los datos de autores de todos los post relacionados en una sola consulta (ver n+1 problem)</p>
<p>
    El segundo parámetro corresponde al nombre de la propiedad "author" de la entidad Post
</p>
<p>Se registra el resolver en el método getResolvers de la clase  AppModule</p>

<CodeBlock>
    function getResolvers(): array
    <CodeBlockOpenChar />
        return [
            &apos;Author::fullName&apos; =&gt; function ($root, $args, $context, $info) <CodeBlockOpenChar />
                return $root[&quot;firstName&quot;] . &quot; &quot; . $root[&quot;lastName&quot;] ?? &apos;&apos;;
            <CodeBlockEndChar />,
            &apos;Post::author&apos; =&gt; PostResolversFactory::getAuthorResolver()
        ];
    <CodeBlockEndChar />

</CodeBlock>
<h3>Crear resolver para obtener los datos de los post desde el autor</h3>
<CodeBlock>
    &lt;?php

namespace AppModule\Graphql;

use AppModule\Entities\Author;
use AppModule\Entities\Post;
use GPDCore\Library\ResolverFactory;

class AuthorResolversFactory
<CodeBlockOpenChar />

    public static function getPostResolver()
    <CodeBlockOpenChar />
        $resolver = ResolverFactory::createCollectionResolver(Author::class, &apos;posts&apos;, null, Post::class);
        return $resolver;
    <CodeBlockEndChar />
<CodeBlockEndChar />
</CodeBlock>
<p>En este caso se utiliza ResolverFactory::createCollectionResolver debido a que se recupera una lista de elementos en lugar de solo uno, en este caso cada registro de autor recupera una lista de objetos Post</p>

<ul>
    
    <li>El nombre de la clase u objeto principal Author::class</li>
    <li>El nombre de la propiedad que tiene los elemento relacionados  (“post”)</li>
    <li>La lista de relaciones ManyToOne o OneToOne de la entidad principal (opcional)</li>
    <li>La clase relacionada con la propiedad de los elementos relacionados que se van a consultar “Post::class” (opcional)</li>
</ul>
<p>

    Los últimos 2 parámetros son opcionales pero para el correcto funcionamiento es necesario asignar uno de ellos, se recomienda poner null en el tercer parámetro y asignar valor al último parámetro.
</p>
<p>

    Se agrega el resolver a la función getResolvers de la clase AppModule
</p>
<CodeBlock>
    function getResolvers(): array
    <CodeBlockOpenChar />
        return [
            &apos;Author::fullName&apos; =&gt; function ($root, $args, $context, $info) <CodeBlockOpenChar />
                return $root[&quot;firstName&quot;] . &quot; &quot; . $root[&quot;lastName&quot;] ?? &apos;&apos;;
            <CodeBlockOpenChar />,
            &apos;Post::author&apos; =&gt; PostResolversFactory::getAuthorResolver(),
            &apos;Author::posts&apos; =&gt; AuthorResolversFactory::getPostResolver()
        ];
    <CodeBlockOpenChar />

</CodeBlock>

<p>Para organizar mejor el código se puede pasar la funcionalidad del resolver Author::fullName a un método de la clase AuthorResolversFactory</p>
<CodeBlock>
    &lt;?php

namespace AppModule\Graphql;

use AppModule\Entities\Author;
use AppModule\Entities\Post;
use GPDCore\Library\ResolverFactory;

class AuthorResolversFactory
<CodeBlockOpenChar />

    public static function getPostResolver()
    <CodeBlockOpenChar />
        $resolver = ResolverFactory::createCollectionResolver(Author::class, &apos;posts&apos;, null, Post::class);
        return $resolver;
    <CodeBlockEndChar />

    public static function getFullnameResolver(): callable
    <CodeBlockOpenChar />
        return function ($root, $args, $context, $info) <CodeBlockOpenChar />
            return $root[&quot;firstName&quot;] . &quot; &quot; . $root[&quot;lastName&quot;] ?? &apos;&apos;;
        <CodeBlockEndChar />;
    <CodeBlockEndChar />
<CodeBlockEndChar />

</CodeBlock>
<p>Y se actualiza en AppModule</p>
<CodeBlock>

    
  function getResolvers(): array
  <CodeBlockOpenChar />
      return [
          &apos;Post::author&apos; =&gt; PostResolversFactory::getAuthorResolver(),
          &apos;Author::posts&apos; =&gt; AuthorResolversFactory::getPostResolver(),
          &apos;Author::fullName&apos; =&gt; AuthorResolversFactory::getFullnameResolver(),
      ];
  <CodeBlockEndChar />

</CodeBlock>
</PageContent>
