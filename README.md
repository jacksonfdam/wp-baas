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



## Referencias

https://blog.apiki.com/wp-rest-api-content-endpoints-wordpress-4-7/
https://medium.com/the-lab-of-codes/wordpress-sem-php-bem-vindo-ao-conceito-de-api-156cb80de167
http://www.matera.com/blog/post/como-customizar-o-response-da-api-rest-do-wordpress
https://devblog.drall.com.br/como-acessar-a-rest-api-json-do-wordpress
https://make.wordpress.org/core/2015/10/28/rest-api-welcome-the-infrastructure-to-core/
https://www.hostinger.com.br/tutoriais/guia-iniciante-api-rest-wordpress/
https://tableless.com.br/rest-json-wp-api-e-o-futuro-do-wordpress/
https://developer.wordpress.org/rest-api/extending-the-rest-api/
https://www.wpsuperstars.net/wordpress-rest-api/
https://developer.wordpress.org/rest-api/reference/
https://betomuniz.com/blog/cors-e-alem/
https://testautomationresources.com/api-testing/differences-web-services-api/
https://blog.vertigo.com.br/o-que-e-api-entenda-de-uma-maneira-simples/
https://www.chromium.org/Home/chromium-security/corb-for-developers
https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Controle_Acesso_CORS
https://developer.mozilla.org/en-US/docs/Glossary/same-origin_policy
https://www.kaspersky.com.br/blog/35c3-spectre-meltdown-2019/11289/
https://pt.wikipedia.org/wiki/Cross-site_request_forgery
https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types
https://betomuniz.com/blog/cors-e-alem/
https://www.tecmundo.com.br/programacao/1807-o-que-e-api-.htm
https://blog.algaworks.com/4-conceitos-sobre-rest-que-qualquer-desenvolvedor-precisa-conhecer/
https://sensedia.com/blog/apis/o-que-sao-apis-parte-1-introducao/
https://scriptcerto.com.br/blogwordpress/como-usar-a-api-http-do-wordpress/
https://gist.github.com/danielpataki/c7e98b9596bfb6d085b0#file-simple-get-php
https://www.sitepoint.com/the-wordpress-http-api/
https://devblog.drall.com.br/como-acessar-a-rest-api-json-do-wordpress
https://www.isbrasil.info/blog/criando-uma-api-rest-para-wordpress.html
