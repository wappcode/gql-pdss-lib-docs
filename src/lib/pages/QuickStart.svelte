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
</PageContent>
