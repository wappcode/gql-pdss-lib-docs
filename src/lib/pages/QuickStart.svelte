<script>
	import { base } from '$app/paths';
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

	<h2>Configuración de la Base de Datos</h2>

	<p>
		Antes de comenzar, asegúrate de tener MySQL configurado y crea una base de datos para tu
		aplicación.
	</p>

	<h3>Crear el Archivo de Configuración de Doctrine</h3>

	<p>
		Crea un archivo llamado <strong><code>config/doctrine.local.php</code></strong> y agrega el siguiente
		código:
	</p>

	<CodeBlock>
		&lt;?php return [ &apos;driver&apos; =&gt; [ &apos;user&apos; =&gt; &apos;root&apos;,
		&apos;password&apos; =&gt; &apos;rootpassword&apos;, &apos;driver&apos; =&gt;
		&apos;pdo_mysql&apos;, &apos;host&apos; =&gt; &apos;localhost&apos;, &apos;dbname&apos; =&gt;
		&apos;&apos;, &apos;charset&apos; =&gt; &apos;utf8mb4&apos; ], &apos;entities&apos; =&gt;
		require __DIR__ . &apos;/doctrine.entities.php&apos; ];
	</CodeBlock>

	<p>
		Asegúrate de reemplazar los siguientes valores con la información correspondiente a tu servidor
		de base de datos:
	</p>
	<ul>
		<li><strong>'user':</strong> El nombre de usuario de tu base de datos.</li>
		<li><strong>'password':</strong> La contraseña de tu base de datos.</li>
		<li><strong>'host':</strong> La dirección del servidor de la base de datos.</li>
		<li><strong>'dbname':</strong> El nombre de la base de datos que has creado.</li>
	</ul>
	<p>
		Si estás utilizando una base de datos que no sea MySQL, ajusta el valor de 'driver' según
		corresponda.
	</p>

	<hr />

	<h1>Permisos de Escritura para la Carpeta "data"</h1>
	<p>
		Para asegurar el correcto funcionamiento de la aplicación, es importante otorgar permisos de
		escritura a la carpeta "data". A continuación, se detallan los pasos para hacerlo en diferentes
		sistemas operativos:
	</p>

	<h2>Linux:</h2>
	<p>
		En sistemas Linux, puedes usar el comando <code>chmod</code> para cambiar los permisos de la carpeta
		"data" y otorgar permisos de escritura. Abre tu terminal y ejecuta el siguiente comando:
	</p>
	<pre><code>chmod a+w data</code></pre>
	<p>Esto permitirá que cualquier usuario en el sistema pueda escribir en la carpeta "data".</p>

	<h2>Windows:</h2>
	<p>
		En sistemas Windows, puedes cambiar los permisos de la carpeta "data" a través de la interfaz
		gráfica del explorador de archivos:
	</p>
	<ol>
		<li>Navega hasta la carpeta "data".</li>
		<li>Haz clic derecho sobre la carpeta y selecciona "Propiedades".</li>
		<li>Ve a la pestaña "Seguridad".</li>
		<li>Haz clic en "Editar" y luego en "Agregar".</li>
		<li>Escribe "Todos" en el campo de nombres de objetos y haz clic en "Comprobar nombres".</li>
		<li>Haz clic en "Aceptar" para cerrar la ventana de selección de usuarios.</li>
		<li>
			Selecciona el usuario "Todos" en la lista de nombres de grupos o usuarios y marca la casilla
			"Control total" en la columna "Permitir".
		</li>
		<li>Haz clic en "Aplicar" y luego en "Aceptar" para aplicar los cambios.</li>
	</ol>
	<p>Esto otorgará permisos de escritura a todos los usuarios en la carpeta "data".</p>

	<h2>macOS:</h2>
	<p>
		En macOS, puedes usar el comando <code>chmod</code> de manera similar a como se hace en Linux. Abre
		la Terminal y ejecuta el siguiente comando:
	</p>
	<pre><code>chmod a+w data</code></pre>
	<p>Esto permitirá que cualquier usuario en el sistema pueda escribir en la carpeta "data".</p>

	<p>
		<strong
			>Recuerda en entornos de producción aplicar las mejores prácticas de seguridad y otorgar
			permisos solo a los grupos y usuarios relevantes.</strong
		>
	</p>

	<hr />

	<h1>Crear Entidad Author (Autor)</h1>
	<p>
		Para definir una entidad Doctrine que represente un autor, crea un nuevo archivo <code
			>Author.php</code
		>
		en la siguiente ubicación: <code>modules/AppModule/src/Entities/Author.php</code> y agrega el siguiente
		código:
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
		La clase <code>Author</code> es una entidad Doctrine que utiliza anotaciones para definir sus
		columnas en la base de datos. Esta clase extiende la clase <code>AbstractEntityModel</code>, la
		cual agrega las propiedades <code>id</code> (clave primaria), <code>created</code> y
		<code>updated</code>.
	</p>

	<p>
		Los métodos <code>get</code> se utilizan para generar los campos tipo objeto GraphQL. Para que funcionen
		correctamente, estos métodos deben ser públicos y tener un tipo de retorno asignado.
	</p>

	<p>
		Los métodos <code>set</code> se utilizan para generar los tipos de entrada de GraphQL. Deben ser
		públicos y tener asignado un tipo requerido.
	</p>

	<p>
		Para generar el código SQL necesario para actualizar la base de datos, ejecuta el siguiente
		comando en la terminal:
	</p>
	<pre><code>vendor/bin/doctrine orm:schema-tool:update --dump-sql</code></pre>

	<p>Para aplicar los cambios en la base de datos, utiliza el siguiente comando:</p>
	<pre><code>vendor/bin/doctrine orm:schema-tool:update --force</code></pre>

	<hr />

	<h1>Registrar un Campo GraphQL para Crear Autores</h1>
	<p>
		Para registrar un campo GraphQL que permita la creación de autores, es necesario crear la clase <code
			>AppModule</code
		>
		para que extienda la clase <code>GPDCore\Library\AbstractModule</code> e implemente los siguientes
		métodos:
	</p>

	<CodeBlock>
		use GPDCore\Library\AbstractModule; use GPDCore\Graphql\GPDFieldFactory; use
		AppModule\Entities\Author; class AppModule extends AbstractModule
		<CodeBlockOpenChar />
		// Otros métodos y propiedades de la clase AppModule public function getConfig(): array
		<CodeBlockOpenChar />
		// Implementación para obtener la configuración de la aplicación
		<CodeBlockEndChar />

		public function getServicesAndGQLTypes(): array
		<CodeBlockOpenChar />
		// Implementación para obtener los servicios y tipos GraphQL de la aplicación
		<CodeBlockEndChar />

		public function getResolvers(): array
		<CodeBlockOpenChar />
		// Implementación para obtener los resolutores de la aplicación
		<CodeBlockEndChar />

		public function getQueryFields(): array
		<CodeBlockOpenChar />
		// Implementación para obtener los campos de consulta GraphQL de la aplicación
		<CodeBlockEndChar />

		public function getMutationFields(): array
		<CodeBlockOpenChar />
		return [ &apos;createAuthor&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class)
		];
		<CodeBlockEndChar />
		<CodeBlockEndChar />
	</CodeBlock>

	<p>
		Con estos cambios, la clase <code>AppModule</code> estará completamente configurada para registrar
		un campo GraphQL que permita la creación de autores en tu aplicación PHP.
	</p>

	<p>
		En el array devuelto por el método <code>getMutationFields</code>, se agrega un campo GraphQL.
		La clave de este campo corresponde al nombre que se utilizará en las consultas GraphQL. El valor
		asociado a esta clave es un objeto GraphQL creado mediante la clase <code>GPDFieldFactory</code>
		utilizando el método <code>buildFieldCreate</code>.
	</p>

	<h1>Crear un Autor mediante Consulta GraphQL</h1>
	<p>
		Para probar la creación de un autor, la URL de la API depende de la configuración del servidor y
		generalmente apunta al archivo principal en la ruta <code>public/index.php/api</code>.
	</p>

	<p>
		Por ejemplo, en un servidor Apache: <code>http://localhost/my-project/public/index.php/api</code
		>
	</p>

	<p>
		También puedes iniciar un servidor PHP desde el directorio de la aplicación con el siguiente
		comando:
	</p>
	<pre><code>php -S localhost:8000 public/index.php</code></pre>

	<p>De esta manera, la URL sería: <code>http://localhost:8080/api</code></p>

	<p>
		Para realizar la prueba, puedes utilizar una extensión de GraphQL para tu navegador o Postman.
	</p>

	<p>La consulta GraphQL a ejecutar es la siguiente:</p>

	<CodeBlock>
		mutation <CodeBlockOpenChar />
		createAuthor(input: <CodeBlockOpenChar />
		firstName: "John", lastName: "Doe", email: "demo@demo.local.lan"
		<CodeBlockEndChar />) <CodeBlockOpenChar />
		id firstName lastName email created updated
		<CodeBlockEndChar />
		<CodeBlockEndChar /></CodeBlock
	>

	<p>
		Con la extensión GraphQL Playground para Chrome, puedes ejecutar la consulta y visualizar los
		resultados directamente en el navegador.
	</p>

	<p>
		Al ejecutar la consulta, se guardará un nuevo registro en la base de datos. Los campos <code
			>id</code
		>, <code>created</code> y <code>updated</code> son generados automáticamente.
	</p>

	<img src="{base}/images/create-author.png" alt="" />

	<h1>Registrar un Campo GraphQL para Recuperar el Registro de un Autor por ID</h1>
	<p>
		Para registrar un campo GraphQL que recupere el registro de un autor por su ID, actualiza el
		método <code>getQueryFields</code> en el archivo
		<code>modules/AppModule/src/AppModule.php</code> de la siguiente manera:
	</p>

	<CodeBlock
		>function getQueryFields(): array
		<CodeBlockOpenChar />
		return [ &apos;getAuthor&apos; =&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class),
		];
		<CodeBlockEndChar /></CodeBlock
	>

	<p>
		Para probar la obtención del autor por ID, ejecuta el siguiente query en la extensión de Chrome
		GraphQL Playground:
	</p>

	<CodeBlock>
		query <CodeBlockOpenChar />
		getAuthor(id: "1") <CodeBlockOpenChar />
		id firstName lastName email created updated
		<CodeBlockEndChar />
		<CodeBlockEndChar /></CodeBlock
	>
	<p>
		<img src="{base}/images/get-author.png" alt="" />
	</p>
	<p>
		Para hacer más útil la consulta del autor, podemos agregar el nombre completo (<code
			>fullName</code
		>). Para ello, primero registramos un método <code>getFullName</code> en la clase
		<code>Author</code>:
	</p>

	<CodeBlock
		>/** * Obtiene el nombre completo. * * @return string */ public function getFullName()
		<CodeBlockOpenChar />
		return $this->firstName . " " . $this->lastName ?? "";
		<CodeBlockEndChar /></CodeBlock
	>

	<p>
		Con esto, podemos recuperar el nombre completo de un autor desde la clase <code>Author</code> en
		PHP, y la propiedad <code>fullName</code> queda registrada en el esquema de GraphQL.
	</p>
	<p><img src="{base}/images/get-author-fullname-schema.png" alt="" /></p>
	<p>
		Para que funcione correctamente, es necesario agregar un resolver en la clase <code>Author</code
		>
		para <code>fullName</code>. Actualiza el método <code>getResolvers</code> en el archivo
		<code>modules/AppModule/src/AppModule.php</code> de la siguiente forma:
	</p>

	<CodeBlock
		>function getResolvers(): array
		<CodeBlockOpenChar />
		return [ 'Author::fullName' => function($root, $args, $context, $info) <CodeBlockOpenChar />
		return $root["firstName"] . " " . $root["lastName"] ?? '';
		<CodeBlockEndChar />
		];
		<CodeBlockEndChar /></CodeBlock
	>

	<p>
		Es importante que la clave del array esté compuesta por el nombre de la clase seguido de dos
		veces dos puntos (<code>::</code>) y el nombre de la propiedad, en este caso,
		<code>Author::fullName</code>.
	</p>

	<p>Ahora puedes realizar la consulta solicitando el valor de <code>fullName</code>.</p>

	<p><img src="{base}/images/get-author-fullname.png" alt="" /></p>

	<h1>Registrar un Campo GraphQL para Recuperar la Lista de Autores Registrados</h1>
	<p>
		Para obtener la lista de autores registrados, actualiza el método <code>getQueryFields</code> en
		el archivo <code>modules/AppModule/src/AppModule.php</code> de la siguiente manera:
	</p>

	<CodeBlock
		>function getQueryFields(): array
		<CodeBlockOpenChar />
		return [ &apos;getAuthor&apos; =&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class),
		&apos;getAuthorsList&apos; =&gt; GPDFieldFactory::buildFieldList($this-&gt;context, Author::class),
		];
		<CodeBlockEndChar />
	</CodeBlock>

	<p>Ahora puedes realizar consultas GraphQL para recuperar todos los autores registrados.</p>

	<p>
		Antes de realizar la prueba, puedes crear más autores utilizando la entrada <code
			>createAuthor</code
		> para poder visualizar mejor el resultado.
	</p>

	<p>Para probar, ejecuta el siguiente query en la extensión GraphQL:</p>

	<CodeBlock
		>query <CodeBlockOpenChar />
		getAuthorsList <CodeBlockOpenChar />
		fullName firstName lastName id created updated
		<CodeBlockEndChar />
		<CodeBlockEndChar /></CodeBlock
	>
	<p><img src="{base}/images/authors-list.png" alt="" /></p>

	<p>
		La consulta <code>getAuthorsList</code> ya incluye opciones para ordenar y filtrar. Por ejemplo,
		para filtrar los resultados por nombre, puedes ejecutar la siguiente consulta:
	</p>

	<CodeBlock
		>query <CodeBlockOpenChar />
		getAuthorsList(filter: [
		<CodeBlockOpenChar />
		conditions: [
		<CodeBlockOpenChar />
		filterOperator: LIKE, value: <CodeBlockOpenChar />
		single: "%john%"
		<CodeBlockEndChar />, property: "firstName"
		<CodeBlockEndChar />
		]
		<CodeBlockEndChar />
		]) <CodeBlockOpenChar />
		fullName firstName lastName id created updated
		<CodeBlockEndChar />
		<CodeBlockEndChar /></CodeBlock
	>
	<img src="{base}/images/authors-list-filter.png" alt="" />

	<p>Y para ordenar por nombre, puedes ejecutar el siguiente query:</p>

	<CodeBlock>
		query <CodeBlockOpenChar />
		getAuthorsList(sorting: [
		<CodeBlockOpenChar />
		property: "firstName", direction: desc
		<CodeBlockEndChar />
		]) <CodeBlockOpenChar />
		fullName firstName lastName id created updated
		<CodeBlockEndChar />
		<CodeBlockEndChar /></CodeBlock
	>

	<p><strong>NOTA:</strong> Puedes utilizar filtros y orden en la misma consulta.</p>

	<h1>Registrar un Campo GraphQL para Recuperar la Lista Paginada de Autores</h1>
	<p>
		Antes de agregar la entrada para consultar la lista paginada, es necesario registrar los tipos <code
			>connection</code
		>
		y <code>Edge</code> para el tipo de dato que se va a consultar, en este caso
		<code>Author</code>.
	</p>

	<p>
		En la carpeta <code>modules/AppModule/src/Graphql</code>, crea el archivo
		<code>AuthorEdgeFactory.php</code> con el siguiente código:
	</p>

	<CodeBlock
		>&lt;?php namespace AppModule\Graphql; use GPDCore\Library\AbstractEdgeTypeServiceFactory; class
		AuthorEdgeFactory extends AbstractEdgeTypeServiceFactory <CodeBlockOpenChar />

		const NAME = 'AuthorEdge'; const DESCRIPTION = ''; protected static $instance = null;
		<CodeBlockEndChar />
	</CodeBlock>

	<p>
		Esta clase extiende a <code>AbstractEdgeTypeServiceFactory</code>, que tiene los métodos
		necesarios para construir el objeto GraphQL <code>AuthorEdge</code>.
	</p>

	<p>
		La constante <code>NAME</code> corresponde al nombre del objeto GraphQL que se va a crear y no
		debe contener caracteres especiales ni espacios en blanco. La constante <code>DESCRIPTION</code>
		corresponde a la descripción del objeto GraphQL que se va a crear. La propiedad estática
		<code>$instance</code> es importante porque es donde se almacenará la instancia del objeto GraphQL
		una vez que se haya creado, de esta forma solo se creará un solo objeto de este tipo; las siguientes
		solicitudes para obtener el objeto retornarán el objeto creado previamente.
	</p>

	<p>
		Todos los objetos GraphQL deben ser registrados como servicios. Para crear y registrar como
		servicio el objeto <code>AuthorEdge</code>, actualiza el método
		<code>getServicesAndGQLTypes</code>
		de la clase <code>AppModule</code> en el archivo
		<code>modules/AppModule/src/AppModule.php</code> de la siguiente forma:
	</p>

	<CodeBlock
		>function getServicesAndGQLTypes(): array
		<CodeBlockOpenChar />
		return [ &apos;invokables&apos; =&gt; [], &apos;factories&apos; =&gt; [ AuthorEdgeFactory::NAME =&gt;
		AuthorEdgeFactory::getFactory($this-&gt;context, Author::class) ], &apos;aliases&apos; =&gt; [] ];
		<CodeBlockEndChar /></CodeBlock
	>

	<p>
		En el array <code>factories</code>, se agrega la clave con el nombre del nuevo objeto (<code
			>AuthorEdgeFactory::NAME</code
		>), y como valor se agrega el retorno del método <code>AuthorEdgeFactory::getFactory</code> con
		los parámetros de contexto (<code>$this->context</code>) y la clase de la entidad relacionada (<code
			>Author::class</code
		>).
	</p>

	<p>Para el objeto <code>AuthorConnection</code>, se realiza un proceso similar:</p>

	<p>
		En la carpeta <code>modules/AppModule/src/Graphql</code>, crea el archivo
		<code>AuthorConnectionFactory.php</code> con el siguiente código:
	</p>

	<CodeBlock
		>&lt;?php namespace AppModule\Graphql; use GPDCore\Library\AbstractConnectionTypeServiceFactory;
		class AuthorConnectionFactory extends AbstractConnectionTypeServiceFactory <CodeBlockOpenChar />
		const NAME = 'AuthorConnection'; const DESCRIPTION = ''; protected static $instance = null;
		<CodeBlockEndChar />
	</CodeBlock>

	<p>
		Las constantes <code>NAME</code>, <code>DESCRIPTION</code> y <code>$instance</code> tienen la
		misma funcionalidad que en <code>AuthorEdgeFactory</code>.
	</p>

	<p>Se agrega como servicio en la clase <code>AppModule</code>:</p>

	<CodeBlock
		>function getServicesAndGQLTypes(): array
		<CodeBlockOpenChar />
		return [ &apos;invokables&apos; =&gt; [], &apos;factories&apos; =&gt; [ AuthorEdgeFactory::NAME =&gt;
		AuthorEdgeFactory::getFactory($this-&gt;context, Author::class), AuthorConnectionFactory::NAME =&gt;
		AuthorConnectionFactory::getFactory($this-&gt;context, AuthorEdgeFactory::NAME) ] ];
		<CodeBlockEndChar /></CodeBlock
	>

	<p>
		Para este caso, a <code>AuthorConnectionFactory::getFactory</code> se le pasan las propiedades
		de contexto y el nombre del objeto Edge relacionado, en este caso
		<code>AuthorEdgeFactory::NAME</code>
		(<code>"AuthorEdge"</code>).
	</p>

	<p>
		Siempre es importante recordar que para importar otras clases es necesario agregar los
		namespaces al inicio del archivo. Por ejemplo, en el archivo <code>AppModule</code>, después de
		la línea <code>namespace AppModule;</code>, se agregan las líneas:
	</p>

	<CodeBlock
		>use AppModule\Graphql\AuthorConnectionFactory; use AppModule\Graphql\AuthorEdgeFactory;</CodeBlock
	>

	<p>
		Una vez creados los tipos <code>AuthorEdge</code> y <code>AuthorConnection</code>, se puede
		actualizar el método <code>getQueryFields</code> de la clase <code>AppModule</code> para agregar
		la consulta paginada:
	</p>

	<CodeBlock
		>function getQueryFields(): array
		<CodeBlockOpenChar />

		$authorConnection =
		$this-&gt;context-&gt;getServiceManager()-&gt;get(AuthorConnectionFactory::NAME); return [
		&apos;echo&apos; =&gt; [ &apos;type&apos; =&gt; Type::nonNull(Type::string()), &apos;args&apos;
		=&gt; [ &apos;message&apos; =&gt; Type::nonNull(Type::string()) ], &apos;resolve&apos; =&gt;
		function ($root, $args) &lt;CodeBlockOpenChar /&gt; return $args[&quot;message&quot;];
		&lt;CodeBlockEndChar /&gt; ], &apos;getAuthor&apos; =&gt;
		GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class), &apos;getAuthorsList&apos;
		=&gt; GPDFieldFactory::buildFieldList($this-&gt;context, Author::class),
		&apos;getAuthorsPaginatedList&apos; =&gt;
		GPDFieldFactory::buildFieldConnection($this-&gt;context, $authorConnection, Author::class), ];
		<CodeBlockEndChar /></CodeBlock
	>

	<p>El código</p>

	<CodeBlock>
		$authorConnection =
		$this-&gt;context-&gt;getServiceManager()-&gt;get(AuthorConnectionFactory::NAME);
	</CodeBlock>

	<p>
		recupera el objeto GraphQL <code>AuthorConnection</code> desde el administrador de servicios y la
		línea
	</p>

	<CodeBlock>
		&apos;getAuthorsPaginatedList&apos; =&gt;
		GPDFieldFactory::buildFieldConnection($this-&gt;context, $authorConnection, Author::class),
	</CodeBlock>

	<p>es donde se agrega la entrada para consultar autores de forma paginada.</p>

	<p>Para probarlo, se puede ejecutar el siguiente query:</p>

	<CodeBlock
		>query <CodeBlockOpenChar />
		pagination <CodeBlockOpenChar />
		first: 1
		<CodeBlockEndChar />
		<CodeBlockEndChar /></CodeBlock
	>
	<p><img src="{base}/images/authors-list-pagination.png" alt="" /></p>
	<p>
		En este caso, se utiliza el input para recuperar las primeras <code>n</code> registros. El resultado
		contiene la siguiente información:
	</p>

	<ul>
		<li><code>totalCount</code>: Total de elementos.</li>
		<li><code>pageInfo</code>: Información de la página.</li>
		<li>
			<code>edges</code>: En la propiedad <code>nodes</code>, se encuentran los datos de los
			registros consultados.
		</li>
	</ul>

	<p>
		Al igual que la consulta de listas, se tiene la opción para filtrar y ordenar los elementos.
	</p>

	<h1>Registrar un campo graphql para actualizar autores.</h1>
	<p>
		En el archivo modules/AppModule/src/AppModule.php actuallizar el método getMutationFields para
		agregar la entrada 'updateAuthor' de la siguiente forma.
	</p>

	<CodeBlock>
		function getMutationFields(): array
		<CodeBlockOpenChar />
		return [ &apos;createAuthor&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class),
		&apos;updateAuthor&apos; =&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Author::class),
		];
		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		Probar funcionamiento: En la extensión del navegador para graphql ejecutar la siguiente consulta
	</p>

	<CodeBlock>
		mutation UpdateAuthor<CodeBlockOpenChar />
		updateAuthor(id:&quot;1&quot;, input:<CodeBlockOpenChar />
		email: &quot;Email Actualizado&quot;
		<CodeBlockEndChar />)<CodeBlockOpenChar />
		id fullName email
		<CodeBlockEndChar />
		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		Para las actualizaciones los inputs son parciales lo que significa que se puede actualizar ya
		sea todos o solo algunos de datos del registro
	</p>

	<h2>Registrar un campo graphql para eliminar autores.</h2>
	<p>
		En el archivo modules/AppModule/src/AppModule.php actuallizar el método getMutationFields para
		agregar la entrada deleteAuthorde la siguiente forma.
	</p>

	<CodeBlock>
		function getMutationFields(): array
		<CodeBlockOpenChar />
		return [ &apos;createAuthor&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class),
		&apos;updateAuthor&apos; =&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Author::class),
		&apos;deleteAuthor&apos; =&gt; GPDFieldFactory::buildFieldDelete($this-&gt;context, Author::class)
		];
		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		Probar funcionamiento: En la extensión del navegador para graphql ejecutar el siguiente código
	</p>
	<CodeBlock>
		mutation DeleteAuthor<CodeBlockOpenChar />
		deleteAuthor(id:&quot;1&quot;)
		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		Como resultado la consulta retorna un valor boolean (falso o verdadero) indicando si se realizó
		correctamente la operación.
	</p>
	<h3>Crear registro para POST</h3>
	<p>
		Crear archivo Post.php para la entidad en la siguiente ubicación
		modules/AppModule/src/Entities/Post.php
	</p>
	<CodeBlock>
		&lt;?php // modules/AppModule/src/Entities/Post.php namespace AppModule\Entities; use
		Doctrine\ORM\Mapping as ORM; use AppModule\Entities\Author; use
		GPDCore\Entities\AbstractEntityModel; /** * @ORM\Entity * @ORM\Table(name=&quot;posts&quot;) */
		class Post extends AbstractEntityModel
		<CodeBlockOpenChar />
		/** * * @ORM\Column(type=&quot;string&quot;, length=255, nullable=false, name=&quot;title&quot;)
		* @var string */ private $title; /** * @ORM\Column(type=&quot;text&quot;, nullable=false, name=&quot;body&quot;)
		* @var string */ private $body; /** * @ORM\ManyToOne(targetEntity=&quot;\AppModule\Entities\Author&quot;,
		inversedBy=&quot;posts&quot;) * @ORM\JoinColumn(name=&quot;author_id&quot;, referencedColumnName=&quot;id&quot;,
		nullable=false) * @var Author */ private $author; /** * Get the value of title * * @return string
		*/ public function getTitle()
		<CodeBlockOpenChar />
		return $this-&gt;title;
		<CodeBlockEndChar />

		/** * Set the value of title * * @param string $title * * @return self */ public function
		setTitle(string $title)
		<CodeBlockOpenChar />
		$this-&gt;title = $title; return $this;
		<CodeBlockEndChar />

		/** * Get the value of body * * @return string */ public function getBody()
		<CodeBlockOpenChar />
		return $this-&gt;body;
		<CodeBlockEndChar />

		/** * Set the value of body * * @param string $body * * @return self */ public function
		setBody(string $body)
		<CodeBlockOpenChar />
		$this-&gt;body = $body; return $this;
		<CodeBlockEndChar />

		/** * Get the value of author * * @return Author */ public function getAuthor()
		<CodeBlockOpenChar />
		return $this-&gt;author;
		<CodeBlockEndChar />

		/** * Set the value of author * * @param Author $author * * @return self */ public function
		setAuthor(Author $author)
		<CodeBlockOpenChar />
		$this-&gt;author = $author; return $this;
		<CodeBlockEndChar />
	</CodeBlock>
	<p>Se agrega a la clase Author la referencia a los posts</p>
	<CodeBlock>
		/** * * @ORM\OneToMany(targetEntity=&quot;\AppModule\Entities\Post&quot;,
		mappedBy=&quot;author&quot;) * @var Collection */ private $posts; /** * Get the value of posts *
		* @return Collection */ public function getPosts(): Collection
		<CodeBlockOpenChar />
		return $this-&gt;posts;
		<CodeBlockEndChar />

		/** * Set the value of posts * @API\Exclude * @param Collection $posts * * @return self */
		public function setPosts(Collection $posts)
		<CodeBlockOpenChar />
		$this-&gt;posts = $posts; return $this;
		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		Como no vamos a crear post desde el registro de autores se agrega a la función setPost la
		anotación @API\Exclude para que ese método no sea considerado para definir el input de la clase
		Author.
	</p>
	<p>
		Es necesario agregar <code>use GraphQL\Doctrine\Annotation as API;</code> en la parte superior de
		la página para que se incluyan los archivos correspondientes
	</p>

	<p>Se ejecuta el comando para actualizar la base de datos</p>
	<code> vendor/bin/doctrine orm:schema-tool:update --force </code>
	<h1>Se agregan en AppModule las entradas para CRUD para Post</h1>

	<p>Crear clase Edge para post</p>

	<CodeBlock>
		&lt;?php namespace AppModule\Graphql; use GPDCore\Library\AbstractEdgeTypeServiceFactory; class
		PostEdgeFactory extends AbstractEdgeTypeServiceFactory
		<CodeBlockOpenChar />

		const NAME = &apos;PostEdge&apos;; const DESCRIPTION = &apos;&apos; protected static $instance =
		null;
		<CodeBlockEndChar />
	</CodeBlock>
	<p>Crear clase Connection para post</p>
	<CodeBlock>
		&lt;?php namespace AppModule\Graphql; use GPDCore\Library\AbstractConnectionTypeServiceFactory;
		class PostConnectionFactory extends AbstractConnectionTypeServiceFactory &lt;CodeBlockOpenChar
		/&gt; const NAME = &apos;PostConnection&apos;; const DESCRIPTION = &apos;&apos;; protected
		static $instance = null; &lt;CodeBlockEndChar /&gt; Registrar los tipos en ServiceManager en el
		m&eacute;todo getServicesAndGQLTypes de la clase AppModule function getServicesAndGQLTypes():
		array &lt;CodeBlockOpenChar /&gt; return [ &apos;invokables&apos; =&gt; [],
		&apos;factories&apos; =&gt; [ AuthorEdgeFactory::NAME =&gt;
		AuthorEdgeFactory::getFactory($this-&gt;context, Author::class), AuthorConnectionFactory::NAME
		=&gt; AuthorConnectionFactory::getFactory($this-&gt;context, AuthorEdgeFactory::NAME),
		PostEdgeFactory::NAME =&gt; PostEdgeFactory::getFactory($this-&gt;context, Post::class),
		PostConnectionFactory::NAME =&gt; PostConnectionFactory::getFactory($this-&gt;context,
		PostEdgeFactory::NAME) ], ];
		<CodeBlockEndChar />
	</CodeBlock>
	<p>Importarlos con</p>
	<CodeBlock>
		use AppModule\Graphql\PostConnectionFactory; use AppModule\Graphql\PostEdgeFactory;
	</CodeBlock>

	<p>
		Agregar las entradas graphql para ver los registros en el método getQueryFields de la clase
		AppModule
	</p>
	<CodeBlock>
		function getQueryFields(): array
		<CodeBlockOpenChar />

		$authorConnection =
		$this-&gt;context-&gt;getServiceManager()-&gt;get(AuthorConnectionFactory::NAME);
		$postConnection =
		$this-&gt;context-&gt;getServiceManager()-&gt;get(PostConnectionFactory::NAME); return [
		&apos;getAuthor&apos; =&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Author::class),
		&apos;getAuthorsList&apos; =&gt; GPDFieldFactory::buildFieldList($this-&gt;context,
		Author::class), &apos;getAuthorsPaginatedList&apos; =&gt;
		GPDFieldFactory::buildFieldConnection($this-&gt;context, $authorConnection, Author::class),
		&apos;getPost&apos; =&gt; GPDFieldFactory::buildFieldItem($this-&gt;context, Post::class),
		&apos;getPostsList&apos; =&gt; GPDFieldFactory::buildFieldList($this-&gt;context, Post::class),
		&apos;getPostsPaginatedList&apos; =&gt; GPDFieldFactory::buildFieldConnection($this-&gt;context,
		$postConnection, Post::class), ];
		<CodeBlockEndChar />
	</CodeBlock>

	<p>
		Agregar las entradas graphql para crear, actualizar y eliminar los registros en el método
		getMutationFields de la clase AppModule
	</p>
	<CodeBlock>
		function getMutationFields(): array
		<CodeBlockOpenChar />
		return [ &apos;createAuthor&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Author::class),
		&apos;updateAuthor&apos; =&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Author::class),
		&apos;deleteAuthor&apos; =&gt; GPDFieldFactory::buildFieldDelete($this-&gt;context, Author::class),
		&apos;createPost&apos; =&gt; GPDFieldFactory::buildFieldCreate($this-&gt;context, Post::class), &apos;updatePost&apos;
		=&gt; GPDFieldFactory::buildFieldUpdate($this-&gt;context, Post::class), &apos;deletePost&apos; =&gt;
		GPDFieldFactory::buildFieldDelete($this-&gt;context, Post::class) ];
		<CodeBlockEndChar />
	</CodeBlock>

	<p>Crear resolver para obtener los datos del autor desde el post</p>

	<p>Se crea la clase PostResolversFactory en la carpeta modules/AppModule/src/Graphql</p>
	<CodeBlock>
		&lt;?php namespace AppModule\Graphql; use AppModule\Entities\Author; use
		GPDCore\Library\EntityBuffer; use GPDCore\Library\ResolverFactory; class PostResolversFactory
		<CodeBlockOpenChar />

		public static function getAuthorResolver()
		<CodeBlockOpenChar />
		$entityBuffer = new EntityBuffer(Author::class); $resolver = ResolverFactory::createEntityResolver($entityBuffer,
		&apos;author&apos;); return $resolver;
		<CodeBlockEndChar />
		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		El método <code>ResolverFactory::createEntityResolver</code> recibe dos parámetros el primero es
		un objeto tipo EntityBuffer que se encarga de optimizar la consulta para obtener los datos de autores
		de todos los post relacionados en una sola consulta (ver n+1 problem)
	</p>
	<p>El segundo parámetro corresponde al nombre de la propiedad "author" de la entidad Post</p>
	<p>Se registra el resolver en el método getResolvers de la clase AppModule</p>

	<CodeBlock>
		function getResolvers(): array
		<CodeBlockOpenChar />
		return [ &apos;Author::fullName&apos; =&gt; function ($root, $args, $context, $info) <CodeBlockOpenChar
		/>
		return $root[&quot;firstName&quot;] . &quot; &quot; . $root[&quot;lastName&quot;] ?? &apos;&apos;;
		<CodeBlockEndChar />, &apos;Post::author&apos; =&gt; PostResolversFactory::getAuthorResolver()
		];
		<CodeBlockEndChar />
	</CodeBlock>
	<p>Crear resolver para obtener los datos de los post desde el autor</p>
	<CodeBlock>
		&lt;?php namespace AppModule\Graphql; use AppModule\Entities\Author; use
		AppModule\Entities\Post; use GPDCore\Library\ResolverFactory; class AuthorResolversFactory
		<CodeBlockOpenChar />

		public static function getPostResolver()
		<CodeBlockOpenChar />
		$resolver = ResolverFactory::createCollectionResolver(Author::class, &apos;posts&apos;, null, Post::class);
		return $resolver;
		<CodeBlockEndChar />
		<CodeBlockEndChar />
	</CodeBlock>

	<p>
		En este caso, se utiliza <code>ResolverFactory::createCollectionResolver</code> porque se recupera
		una lista de elementos en lugar de solo uno. Es importante destacar que cada registro de autor recupera
		una lista de objetos Post.
	</p>
	<p>Los parámetros de esta función son:</p>
	<ol>
		<li>El nombre de la clase u objeto principal Author::class</li>
		<li>El nombre de la propiedad que tiene los elemento relacionados (“post”)</li>
		<li>La lista de relaciones ManyToOne o OneToOne de la entidad principal (opcional)</li>
		<li>
			La clase relacionada con la propiedad de los elementos relacionados que se van a consultar
			“Post::class” (opcional)
		</li>
	</ol>
	<p>
		Los últimos 2 parámetros son opcionales pero para el correcto funcionamiento es necesario
		asignar uno de ellos, se recomienda poner null en el tercer parámetro y asignar valor al último
		parámetro.
	</p>
	<p>Se agrega el resolver a la función getResolvers de la clase AppModule</p>
	<CodeBlock>
		function getResolvers(): array
		<CodeBlockOpenChar />
		return [ &apos;Author::fullName&apos; =&gt; function ($root, $args, $context, $info) <CodeBlockOpenChar
		/>
		return $root[&quot;firstName&quot;] . &quot; &quot; . $root[&quot;lastName&quot;] ?? &apos;&apos;;
		<CodeBlockOpenChar />, &apos;Post::author&apos; =&gt; PostResolversFactory::getAuthorResolver(),
		&apos;Author::posts&apos; =&gt; AuthorResolversFactory::getPostResolver() ];
		<CodeBlockOpenChar />
	</CodeBlock>

	<p>
		Para organizar mejor el código se puede pasar la funcionalidad del resolver Author::fullName a
		un método de la clase AuthorResolversFactory
	</p>
	<CodeBlock>
		&lt;?php namespace AppModule\Graphql; use AppModule\Entities\Author; use
		AppModule\Entities\Post; use GPDCore\Library\ResolverFactory; class AuthorResolversFactory
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
		return [ &apos;Post::author&apos; =&gt; PostResolversFactory::getAuthorResolver(), &apos;Author::posts&apos;
		=&gt; AuthorResolversFactory::getPostResolver(), &apos;Author::fullName&apos; =&gt; AuthorResolversFactory::getFullnameResolver(),
		];
		<CodeBlockEndChar />
	</CodeBlock>
	<p>
		Con todas estas configuraciones establecidas, ahora podemos utilizar la API para realizar una
		variedad de acciones con respecto a los posts: visualizar, crear, actualizar y eliminar.
	</p>
	<p>Además, es posible obtener los datos del autor mediante una consulta del post.</p>
</PageContent>
