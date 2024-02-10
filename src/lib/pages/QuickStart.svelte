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

<code>
	<pre>
        &lt;?php
        return [
            &quot;driver&quot; =&gt; [
                &apos;user&apos;     =&gt;   &apos;root&apos;,
                &apos;password&apos; =&gt;   &apos;rootpassword&apos;,
                &apos;driver&apos;   =&gt;   &apos;pdo_mysql&apos;,
                &apos;host&apos;   =&gt;     &apos;localhost&apos;,
                &apos;dbname&apos;   =&gt;   &apos;&apos;,
                &apos;charset&apos; =&gt;    &apos;utf8mb4&apos;
            ],
            &quot;entities&quot; =&gt; require __DIR__ . &quot;/doctrine.entities.php&quot;
        ];
    
    </pre>
</code>

<p>Reemplazar los datos de user, password y host conforme al servidor de la base de datos.</p>
<p>Si se utiliza una base de datos diferente a mysql ajustar también el valor de driver</p>

<p>
	<strong>Crear Entidad Author (Autor)</strong><br />
	Crear archivo modules/AppModule/src/Entities/Author con el siguiente código /****
</p>

<code>
	<pre>

        &lt;?php
        namespace AppModule\Entities;
        
        use Doctrine\ORM\Mapping as ORM;
        use GPDCore\Entities\AbstractEntityModel;
        
        class Author extends AbstractEntityModel{'{'}
        
        
            /**
             * 
             * @ORM\Column(type=&quot;string&quot;, length=255, nullable=false, name=&quot;first_name&quot;)
             * @var string
             */
            private $firstName;
        
        
            /**
             * @ORM\Column(type=&quot;string&quot;, length=255, nullable=true, name=&quot;last_name&quot;)
             *
             * @var ?string
             */
            private $lastName;
        
            /**
             * @ORM\Column(type=&quot;string&quot;, length=255, nullable=false, name=&quot;email&quot;)
             *
             * @var string
             */
            private $email;
        
        
            /**
             * Get the value of firstName
             *
             * @return  string
             */ 
            public function getFirstName()
            {'{'}
                return $this-&gt;firstName;
            {'}'}
        
            /**
             * Set the value of firstName
             *
             * @param  string  $firstName
             *
             * @return  self
             */ 
            public function setFirstName(string $firstName)
            {'{'}
                $this-&gt;firstName = $firstName;
        
                return $this;
            {'}'}
        
            /**
             * Get the value of lastName
             *
             * @return  ?string
             */ 
            public function getLastName()
            {'{'}
                return $this-&gt;lastName;
            {'}'}
        
            /**
             * Set the value of lastName
             *
             * @param  ?string  $lastName
             *
             * @return  self
             */ 
            public function setLastName(?string $lastName)
            {'{'}
                $this-&gt;lastName = $lastName;
        
                return $this;
            {'}'}
        
            /**
             * Get the value of email
             *
             * @return  string
             */ 
            public function getEmail()
            {'{'}
                return $this-&gt;email;
            {'}'}
        
            /**
             * Set the value of email
             *
             * @param  string  $email
             *
             * @return  self
             */ 
            public function setEmail(string $email)
            {'{'}
                $this-&gt;email = $email;
        
                return $this;
            {'}'}
        
           
        {'}'}
        
    </pre>
</code>
