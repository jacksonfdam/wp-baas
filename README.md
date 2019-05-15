# Mobile Backend como Serviço

Discutiremos a possibilidade de usar o WordPress como um servidor para alimentar conteúdo para aplicativos móveis e armazenar as informações geradas pelos usuários destes.
Para isso, ferramentas disponíveis no ecossistema serão analisadas, ambas oferecidas pelo CMS (API REST, autenticação, banco de dados ...) e geradas pela comunidade ou por nós mesmos (plugins).

Serão estudados quais pontos essenciais dos quais um MBaaS (Mobile Backend como Serviço) deve cobrir WordPress e serão ponderados as ocasiões em que usá-lo com relação a outras soluções do mercado mais comum.

Finalmente, você verá seu uso em um aplicativo real implementado.


## Como instalar WordPress no Docker (Windows, MacOS e Linux)
https://www.hostinger.com.br/tutoriais/como-instalar-wordpress-no-docker-windows-macos-e-linux/

https://upcloud.com/community/tutorials/wordpress-with-docker/
https://upcloud.com/community/tutorials/load-balancing-docker-swarm-mode/


## WordPress em contêiner e suas peculiaridades

https://www.lambda3.com.br/2017/02/migrando-um-blog-do-wordpress-para-um-conteiner-docker/

O WordPress possui um repositório oficial no Docker Hub, e as imagens estão sempre taggeadas de acordo com a versão do WordPress que elas representam. Na home do repo há até indicações de como rodar um contêiner já com o MySql, e até um exemplo com docker-compose. Só que contêineres WordPress são um pouco peculiares. É importante eu fazer essa introdução para que algumas decisões que tomamos fiquem mais claras.


## WordPress Dummy Content Filler Plugins
https://premium.wpmudev.org/blog/lorem-ipsum-top-5-wordpress-dummy-content-filler-text-plugins/
https://www.wpkube.com/add-dummy-content-wordpress-website/

https://codex.wordpress.org/Theme_Unit_Test


## Para todas as requisições que faremos, tome nota dessa instrução
Para visualizar sua primeira requisição, abra o Postman (você pode digitar o endereço no browser diretamente, se você não se importar com um monte de código chapado na tela) e digite na URL (válido para todos os exemplos abaixo):

>http://127.0.0.1:8000/wp-json/wp/v2/REQUISIÇÃO_AQUI.

Onde _127.0.0.1_ é o seu site e _REQUISIÇÃO_AQUI_ será a requisição que vocẽ deseja fazer, dentre as possibilidades:

### Para obter uma lista de Posts  
http://127.0.0.1:8000/wp-json/wp/v2/posts

### Para visualizar um post específico
http://127.0.0.1:8000/wp-json/wp/v2/{id}

Onde {id} é o ID do Post que você precisa.

### Para obter uma lista de Páginas
http://127.0.0.1:8000/wp-json/wp/v2/pages

### Para visualizar uma página especifica:
http://127.0.0.1:8000/wp-json/wp/v2/pages/{id}

Onde {id} é o ID da página que você precisa.

### Parâmetros de paginação 

Qualquer resposta da API que contenha vários recursos suporta vários parâmetros de consulta comuns para manipular a paginação pelos dados da resposta:

-   `?page=`  : especifica a página de resultados a retornar.
    -   Por exemplo,  `/wp/v2/posts?page=2`  é a segunda página de resultados de posts
    -   Ao recuperar  `/wp/v2/posts`  , então  `/wp/v2/posts?page=2`  , e assim por diante, você pode acessar todas as postagens disponíveis através da API, uma página por vez.
-   `?per_page=`  : especifica o número de registros a serem retornados em uma solicitação, especificado como um número inteiro de 1 a 100.
    -   Por exemplo,  `/wp/v2/posts?per_page=1`  retornará apenas o primeiro post da coleção
-   `?offset=`  : especifica um deslocamento arbitrário no qual começar a recuperar postagens
    -   Por exemplo,  `/wp/v2/posts?offset=6`  usará o número padrão de postagens por página, mas começará no 6º post da coleção
    -   `?per_page=5&page=4`  é equivalente a  `?per_page=5&offset=15`


> Consultas grandes podem afetar o desempenho do site, portanto, a  `per_page`  é  **limitada a 100 registros**  .  Se você deseja recuperar mais de 100 registros, por exemplo, para criar uma lista do cliente de todas as categorias disponíveis, poderá fazer várias solicitações de API e combinar os resultados em seu aplicativo.

Para determinar quantas páginas de dados estão disponíveis, a API retorna dois campos de cabeçalho com cada resposta paginada:

-   `X-WP-Total`  : o número total de registros na coleção
-   `X-WP-TotalPages`  : o número total de páginas abrangendo todos os registros disponíveis

Ao inspecionar esses campos de cabeçalho, você pode determinar quanto mais dados estão disponíveis na API.

### Ordenação de Resultados

Além dos parâmetros de consulta de paginação detalhados acima, vários outros parâmetros controlam a ordem dos resultados retornados:

-   `?order=`  : controla se os resultados são retornados em ordem crescente ou decrescente
    -   Os valores válidos são  `?order=asc`  (para ordem ascendente) e  `?order=desc`  (para ordem descendente).
    -   Todas as coleções nativas são retornadas em ordem decrescente por padrão.
-   `?orderby=`  : controla o campo pelo qual a coleção é ordenada
    -   Os valores válidos para  `orderby`  irão variar dependendo do recurso consultado;  Para a coleção  `/wp/v2/posts`  , os valores válidos são "date", "relevant", "id", "include", "title" e "slug"
    -   Todas as coleções com recursos datados o padrão é `orderby=date`


### Para visualizar uma lista de categorias ou tags
Categorias e Tags são consideradas taxonomias. As taxonomias, diferente de posts e páginas, funcionam pelo slug e não pelo ID. O ID é necessário apenas para chamadas de Posts/Páginas.

Os elementos de uma categoria ou tag, são consideradas pelo WordPress como termos (terms). Portanto ao criar a categoria _Marketing_, você está dizendo ao WordPress que a taxonomia Categoria tem um termo chamado _Marketing_.

### Requisitando uma Categoria ou tag de determinado post:

http://127.0.0.1:8000/wp-json/wp/v2/posts/{id_do_post}/terms/category/

http://127.0.0.1:8000/wp-json/wp/v2/posts/{id_do_post}/terms/tag/

### Obtendo uma lista de Categorias
Nossa categoria modelo (Marketing) deverá aparecer aqui

http://127.0.0.1:8000/wp-json/wp/v2/terms/category

### Lista de Tags
http://127.0.0.1:8000/wp-json/wp/v2/terms/tag

### Lidando e visualizando conteúdo com Custom Post Types:

Por padrão, o WP-API não autoriza a visualização direta de CPT. Para autorizar a requisição, abra o arquivo onde você armazena as informações sobre CPT do seu tema (em geral no functions.php, ou qualquer include que você tenha feito) e adicione o argumento (dentro de $args, por favor):

> show_in_rest => true

Caso você tenha feito corretamente, o endereço abaixo vai listar os posts relacionados àquele CPT:

http://127.0.0.1:8000/wp-json/wp/v2/{nome_do_cpt}

### Usando o ACF? Você ainda está na zona de conforto!

A integração com o ACF é extensa para o objetivo do post, o que não importa muito, pois [existe um plugin excelente](https://wordpress.org/plugins/acf-to-wp-api/) que cumpre a maioria das finalidades do ACF. 


### Requer autenticação para todos os pontos requer autenticação para todos os pontos
Você pode exigir autenticação para todas as solicitações da API REST, adicionando uma verificação is_user_logged_in ao filtro rest_authentication_errors :

```php
add_filter( 'rest_authentication_errors', function( $result ) {
    if ( ! empty( $result ) ) {
        return $result;
    }
    if ( ! is_user_logged_in() ) {
        return new WP_Error( 'rest_not_logged_in', 'You are not currently logged in.', array( 'status' => 401 ) );
    }
    return $result;
});
```

### Posso fazer solicitações de API do PHP em um plug-in? 

Sim você pode! Use rest_do_request para fazer solicitações de API internamente em outro código do WordPress:

```php
$request = new WP_REST_Request( 'GET', '/wp/v2/posts' );
// Set one or more request query parameters
$request->set_param( 'per_page', 20 );
$response = rest_do_request( $request );
```

### O que aconteceu com o parâmetro _?filter=_ ?

Quando a API REST foi mesclada no núcleo do WordPress, o parâmetro de consulta ?filter foi removido para evitar futuros problemas de compatibilidade e manutenção. A capacidade de transmitir argumentos wP_Query arbitrários à API usando um parâmetro de consulta ?filter foi necessária na gênese do projeto REST API, mas a maior parte da funcionalidade de filtragem de resposta da API foi substituída por parâmetros de consulta mais robustos como ?categories= ?slug= e ?per_page= .

Parâmetros de consulta primários devem ser usados ​​sempre que possível. No entanto, o plug-in [rest-filter](https://github.com/wp-api/rest-filter) restaura a capacidade de transmitir valores de *?filter* arbitrários na solicitação da API, se necessário.


## Referencias

 - https://blog.apiki.com/wp-rest-api-content-endpoints-wordpress-4-7/
 - https://medium.com/the-lab-of-codes/wordpress-sem-php-bem-vindo-ao-conceito-de-api-156cb80de167
 - http://www.matera.com/blog/post/como-customizar-o-response-da-api-rest-do-wordpress
 - https://devblog.drall.com.br/como-acessar-a-rest-api-json-do-wordpress
 - https://make.wordpress.org/core/2015/10/28/rest-api-welcome-the-infrastructure-to-core/
 - https://www.hostinger.com.br/tutoriais/guia-iniciante-api-rest-wordpress/
 - https://tableless.com.br/rest-json-wp-api-e-o-futuro-do-wordpress/
 - https://developer.wordpress.org/rest-api/extending-the-rest-api/
 - https://www.wpsuperstars.net/wordpress-rest-api/
 - https://developer.wordpress.org/rest-api/reference/
 - https://betomuniz.com/blog/cors-e-alem/
 - https://testautomationresources.com/api-testing/differences-web-services-api/
 - https://blog.vertigo.com.br/o-que-e-api-entenda-de-uma-maneira-simples/
 - https://www.chromium.org/Home/chromium-security/corb-for-developers
 - https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Controle_Acesso_CORS
 - https://developer.mozilla.org/en-US/docs/Glossary/same-origin_policy
 - https://www.kaspersky.com.br/blog/35c3-spectre-meltdown-2019/11289/
 - https://pt.wikipedia.org/wiki/Cross-site_request_forgery
 - https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types
 - https://betomuniz.com/blog/cors-e-alem/
 - https://www.tecmundo.com.br/programacao/1807-o-que-e-api-.htm
 - https://blog.algaworks.com/4-conceitos-sobre-rest-que-qualquer-desenvolvedor-precisa-conhecer/
 - https://sensedia.com/blog/apis/o-que-sao-apis-parte-1-introducao/
 - https://scriptcerto.com.br/blogwordpress/como-usar-a-api-http-do-wordpress/
 - https://gist.github.com/danielpataki/c7e98b9596bfb6d085b0#file-simple-get-php
 - https://www.sitepoint.com/the-wordpress-http-api/
 - https://devblog.drall.com.br/como-acessar-a-rest-api-json-do-wordpress
 - https://www.isbrasil.info/blog/criando-uma-api-rest-para-wordpress.html
 - https://kinsta.com/blog/wordpress-http-api-part-1/