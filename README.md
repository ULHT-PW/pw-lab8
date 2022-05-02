**UNIVERSIDADE LUSÓFONA DE HUMANIDADES E TECNOLOGIAS**

# Lab 8: a minha primeira web app em Django: Portfolio I ⛅

### Objetivo 

* Neste laboratório criará uma primeira aplicação django, para se familiarizar com os conceitos de urls, views, templates e sua linguagem. 

* Será o primeiro laboratório para construção do seu projeto, o seu portfolio. O projeto final será realizado por partes, saindo cada semana e até ao final do semestre um novo enunciado com novas tarefas, em modo de laboratório de aulas práticas, onde irão aplicar aspectos aprendidos na aula dessa semana. 

* O objetivo é (i) terem acompanhamento direto ao projeto semanalmente, e (ii) quando terminarem as aulas encerram o projeto. Aconselhamos vivamente que vão fazendo o trabalho proposto. Na avaliação, além do projeto concluido, será avaliada a vossa regularidade em fazer as propostas semanais.

* Neste laboratório não se deve preocupar ocm o conteúdo, apenas com a forma.
 
* Exercitará a edição dos módulos `urls.py`, `views.py` e a criação de templates HTML com linguagem template.


### Recomendações
* leia uma vez o enunciado. Detalha todos os passos e fornece o código necessário, sendo rápida a sua realização.
* Instale e use o Pycharm  para editar o código de forma fácil (a versão profissional permite usar snippets de código interessantes, pode instalar da mesma forma que o IntelliJ, comprovando que é estudante, pois temos uma licença da universidade). O Pycharm sinaliza os erros. Veja com atenção eventuais mensagens. 

* quando necessário, guie-se pelo projeto que fizemos na aula teórica, que  está disponível no [repo GitHub](https://github.com/ULHT-PW/pw-2022-aula-django-01). 

* se tiver dúvidas, consulte os [slides](https://moodle.ensinolusofona.pt/pluginfile.php/318343/mod_label/intro/pw-04-django-01.pptx) e a documentação do [djangoproject](https://www.djangoproject.com/)

* familiarize-se e use o [glossario](https://moodle.ensinolusofona.pt/pluginfile.php/353336/mod_resource/content/1/PW_glossario.pdf) que terão disponivel no exame.

## 1. Primeiros passos 👶
Vamos nesta secção criar um projeto e aplicação django.
1. Abra a linha de comandos (PowerShell ou cmd) e execute os comandos em baixo a cinzento. 
1. Crie e entre na pasta lab6: `mkdir lab8; cd lab8`
1. Instale o pipenv executando: `pip install pipenv` ou, se tiver problemas com este comando, com `python -m pip install pipenv`
   a. se tiver erro de permissões execute:`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
   b. se não conseguir instalar o pipenv ou der erro, trabalhe sem ambiente virtual (na aula pratica os professores ajudarão a criar o ambiente). Para tal, instale o django sem ambiente virtual (pipenv) com o comando `python -m pip install django`. nessa caso não precisarás de ativar o ambiente, com `pipenv shell`, e podes diretamente correr `django-admin startproject config`  
3. Active o ambiente virtual: `pipenv shell`
4. Instale o django: `pipenv install django`
5. crie um projeto django: `django-admin startproject config .`
6. Migre as base de dados `python manage.py migrate` (falaremos nisto na próxima aula).
7. Lance o projeto para ver se está tudo ok, com o comando `python manage.py runserver`. Clique no hiperlink indicado e abra no seu browser. 
8. Pare o servidor com Ctrl + C
9. Crie a aplicação portfolio, com a instrução `python manage.py startapp portfolio`

 
## 2. Configure a aplicação ⚙️
1. abra a pasta com o Pycharm
1. em `config/settings.py` registe a aplicação na lista `INSTALLED_APPS`, colocando no fim `'portfolio'`
1. em `config/urls.py` registe a rota para a nova aplicação portfolio, inserindo na lista urlpatterns o caminho `path('', include('portfolio.urls))` para a sua aplicação, ficando:

```python
# config/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('portfolio.urls')),
]
```

## 3. Templates 🖺
Designa-se de template um ficheiro HTML retornado ao browser por uma função view específica, eventualmente renderizado com conteúdos. Começamos assim por construir os conteúdos que teremos para retornar a um cliente. Vamos criar um template base\pai que terá o layout, os restantes consistindo em templates "filhos" que herdam e estendem a base, inserindo conteúdos neste.


### 3.1 Template layout.html

1. na pasta `portfolio` crie a pasta `templates`, e dentro dessa a pasta `/portfolio`, ficando com o caminho `lab8/portfolio/templates/portfolio`

1. Crie, na pasta `portfolio/templates/portfolio`, o ficheiro `layout.html`, usando o snippet HTML5 sugerido pelo Pycharm. Será o layout base para a web app que construiremos.
 
O template layout.html que construiremos a seguir terá a seguinte estrutura:
```html
<!-- base.html -->
...
<body>
    <header>...</header>
	<main>...</main>
    <footer>...</footer>
</body>
```

Para já, faça um layout semelhante a este [exemplo](https://codepen.io/LucioStuder/pen/oNprRQd). Deverá personalizar, mas depois; e nessa altura, mudando aqui muda todas as páginas que usem este layout como base!

#### header

1. No `<header>`inclua um elemento `div` onde colocará o título (pode ser seu nome) e um elemento `<nav>` com hiperlinks `<a>` para as páginas que o seu site irá ter. Especifique a formatação num elemento `<style>`. Com `display:flex; justify-content:spacebetween;`, encoste o `div` à esquerda e o `<nav>` à direita. Espace os elementos dentro do `<nav>` de forma igual. Veja o [exemplo](https://codepen.io/LucioStuder/pen/oNprRQd). 

1. Especifique o conteúdo dos hiperlinks do menu. Insira por exemplo `href="{% url 'portfolio:home' %}"`, onde `portfolio` é o nome dado à app (`app_name = 'portfolio'`), e `home` é o nome do path (`name="home"`, a especificar em portfolio/urls.py (veja exemplo da aula [aqui](https://github.com/ULHT-PW/pw-2022-aula-django-01/blob/main/pw/urls.py)).


#### main

1. Por baixo do `<header>`, crie uma secção `<main>`. Contém uma etiqueta template `{% block main %}`, e será estendido com conteúdos por templates filhos. 

```html
<!-- base.html -->
...
<main> 
	{% block main %}
	{% endblock main %}
</main>
```


#### footer

colocar um elemento `div` com ULHT e ano, centrado. colocará mais informação depois.


### 3.2 Templates Filhos
1. Crie templates HTML que estendam o layout.html.
3. Deverão ter a seguinte sintaxe (exemplifica-se para a página `home.html`):

```html
<!-- home.html -->

{% extends 'portfolio/layout.html' %}

{% block main %}
	<h3>Titulo</h3>
    <p>texto texto texto texto texto texto texto </p>
{% endblock %}
```
1. Estes serão os conteúdos que irão aparecer no elemento main. 
2. A única coisa que mudará entre as páginas será o conteúdo do block main.
3. Crie páginas: apresentação, formação, projetos, competencias, 
3. Especifica para cada um deles um título e como texto duas ou tres frases.


## 4. Static 🖼️
A pasta static contém ficheiros "estáticos", i.e., imagens, ficheiros CSS e scripts JavaScript. Estes organizam-se em pastas especificas. Usaremos a seguinte estrutura para guardar uma imagem e um ficheiro css:
```dos
lab8
└───portfolio
    └───static
        └───portfolio
            ├───css
            │       layout.css
            │
            └───images
                    imagem.png	    
```
É extensa, mas previne problemas de ambiguidade.

1. crie a estrutura acima. Na pasta `portfolio` crie a pasta `static`, e dentro dessa a pasta `portfolio`. Esta pasta deverá conter uma pasta `css` e outra `ìmages`. 


### 4.1 CSS
1. Crie dentro da pasta `css` o ficheiro `estilos.css` com algumas configurações.
1. mova para aqui todos os estilos que definiu anteriormente.
1. Inclua no ficheiro `layout.html` um elemento `link` para importar o ficheiro estilos.css. Inclua antes deste a etiqueta template `{% load static %}` e no `href` construa o URL para o path relativo da seguinte forma:

```html
<!-- layout.html -->
...
{% load static %}
<link rel="stylesheet" href="{% static 'portfolio/css/layout.css' %}">
````

### 4.2 images
1. Insira em `portfolio/static/portfolio/images` uma imagem a seu gosto, com uma largura máxima de 200px
1. coloque-a na página `home.html` no elemento `main`, por baixo do texto. A sua referência no `src` recorre igualmente À etiqueta `static` para construir o path relativo:

```html
<!-- home.html -->
...
{% load static %}
<img src="{% static 'portfolio/images/image.png' %}">
```


## 5. Views ⚙️
As views são funções responsáveis por responder ao pedido (request) de um recurso (URL), retornando o recurso pedido, um template HTML eventualmente renderizado com dados e customizado. Fazem assim a interligação entre os dados e os templates, respondendo aos pedidos encaminhados via urls.

1. no ficheiro `views.py` crie uma função view que renderize cada uma das páginas. Por exemplo, para renderizar a página home.html teremos a função `home_page_view`:

```python
#  hello/views.py

from django.shortcuts import render

def home_page_view(request):
	return render(request, 'portfolio/home.html')
```

2. experimente passar como contexto a data, recorrendo ao módulo datetime (de forma semelhante à feita no projeto da aula, veja no [repo GitHub](https://github.com/ULHT-PW/pw-2022-aula-django-01/blob/main/pw/views.py) no módulo views.py), de forma a que esta apareça na pagina `home`.
3. brinque e explore a linguagem de template, com decisores if e ciclos for (veja como fizemos na aula no [index.html](https://github.com/ULHT-PW/pw-2022-aula-django-01/blob/main/pw/templates/pw/index.html), e consulte os [slides](https://moodle.ensinolusofona.pt/pluginfile.php/318343/mod_label/intro/pw-04-django-01.pdf#page=49) para explorar mais variantes. 


## 6. URLS ✉️
Existem dois ficheiros urls. O urls.py da pasta config, responsável por encaminhar um pedido de um recurso à respetiva aplicação (no nosso caso temos as aplicações admin  e a nossa, portfolio). E também deverá existir um módulo urls.py na pasta portfolio. Este irá mapear, para um determinado pedido (*request*) dum recurso, uma função do ficheiro views.py que tratará desse pedido, preparando e devolvendo o recurso pedido, um ficheiro HMTL.

1. o config/urls.py já foi configurado na secção 1.2

3. Na pasta portfolio crie o ficheiro `urls.py`. Exemplifica-se em baixo uma rota na lista urlpatterns, devendo incluir uma rota para cada uma das views anteriormente criadas. 

```python
#  hello/urls.py

from django.shortcuts import render
from . import views

app_name = "portfolio"

urlpatterns = [
    path('home', views.home_page_view, name='home')
]
```
Como se vê, este módulo importa o módulo views que se encontra na mesm *package*/pasta (e por isso é importado como `from . import views`), por forma a poder referir funções de views. Importa também a função path, responsavel por mapear a rota (`home`) na função (`views.home_page_view`).



## 7. Recapitulando links 🔗

Recapitulando links de hiperlinks, imagens e ficheiros css:

1. Para criar **hiperlinks**, insira `href="{% url 'portfolio:home' %}"`, onde `portfolio` é o nome dado à app (no ficheiro `portfolio\urls.py` deverá ter `app_name=portfolio`), e `home` é o nome do path especificado em portfolio/urls.py. 

2. Para a **imagem** `<img>` no ficheiro `base.html`, inclua antes desta a etiqueta template `{% load static %}`, para construir o URL para o path relativo. Na especificação da `src`, use a etiqueta template `{% static 'portfolio/images/image.png' %}`, ficando da seguinte forma:

```html
<!-- base.html -->
...
{% load static %}
<img src="{% static 'portfolio/images/image.png' %}">
```
3. para o **ficheiro CSS** base.css, devemos também incluir no ficheiro `base.html` um link, usando o path relativo para a pasta static:
```html
<!-- base.html -->
...
{% load static %}
<link rel="stylesheet" href="{% static 'portfolio/css/base.css' %}">
```


## 8. Ready... GO! 🏁
1. Lance a aplicação com o comando `python manage.py runserver` e verifique que consegue visualizar corretamente a aplicação que fez.

1. Uma vez tendo a aplicaçao base a funcionar, pode a) adicionar mais páginas HTML, b) referi-las no menu através de hiperlinks, c) criar em `views.py` funções views que as retornem, d) e criar em `urls.py` paths que as mapeiem.


## 9. Página Licenciatura
1. licenciatura será uma pagina do seu portfolio que apresenta a lista de cadeiras do seu curso, organizada por semestre e anos. Deverá apresentar os seguintes atributos: nome, ano, semestre, ECTS, ano letivo frequentado, topicos abordados, ranking de 1 a 5 estrelas (indicando se gostou ou não), professores (da classe Pessoa com campos nome e link para a sua pagina da lusofona e no linkedin), link para página da cadeira (se existir), lista de projetos realizados (classe projeto)
1. Construa em HTML & CSS uma pagina licenciatura que liste 3 cadeiras, formatando devidamente os atributos identificados (para já, podem ter texto inventado). Esse template CSS poderá depois ser replicado nas restantes páginas.
1. use por exemplo este formato de [items isolados](https://codepen.io/LucioStuder/pen/MWrKXdy), ou como [aqui](https://github.com/ULHT-PW/pw-lab4#1-capitais-europeias-com-flexbox) com imagens

## 10. Página Projetos
1. a página terá uma lista de projetos realizados, com atributos: titulo, descrição até 500 carateres, imagem, ano de realização, cadeira (classe Cadeira, caso tenha sido projeto associado a uma cadeira), participantes (da classe Pessoa, da classe Pessoa com atributos nome e link para a sua pagina no linkedin, e link para a aplicação portfolio do projeto PW), link para repositorio GitHub, link para video no youtube, tecnologias usadas, competencias (classe Competencia)
1. Construa em HTML & CSS uma pagina projetos que liste 3 projetos, formatando devidamente os atributos identificados (para já, podem ter texto inventado). Esse template CSS poderá depois ser replicado nas restantes páginas.
2. use por exemplo este formato de [items isolados](https://codepen.io/LucioStuder/pen/MWrKXdy), ou como [aqui](https://github.com/ULHT-PW/pw-lab4#1-capitais-europeias-com-flexbox) com imagens


## 11. Hero Page 🦸‍♀️

1. crie uma hero page, página de entrada com alguns elementos por baixo, a sua "carta de apresentação" (lembre-se que um visitante cria uma opinião sobre um site em menos de 1 segundo!).

2. Utilize a que criou como entrada da aplicação Heroku, com o indice dos laboratórios de PW. O princípio é fazer algo ao estilo deste [exemplo](https://codepen.io/LucioStuder/pen/vYpqwra) 
   1. uma fotografia ou video em background
   1. uma frase que goste ao lado, motivacional
   1. manter o menu anterior em cima. 

3. Se quiser, reviste, mas sem perder muito tempo, ideias dos slides sobre [web design]( https://moodle.ensinolusofona.pt/pluginfile.php/318343/mod_label/intro/pw-02.8-web-design.pdf#page=40) pgs 41 a 46 e os slides sobre [efeitos e animações]( https://moodle.ensinolusofona.pt/pluginfile.php/318343/mod_label/intro/pw-02.10-efeitos-e-animacoes.pdf?time=1648059790707) (video-background, parallax), e o [lab4]( https://github.com/ULHT-PW/pw-lab4) e [lab 5]( https://github.com/ULHT-PW/pw-lab5) que fez.
   
4. crie um elemento `<article>` por baixo, que apareça se fizer scroll, À semelhança do [exemplo](https://codepen.io/LucioStuder/pen/vYpqwra?editors=1100), e inclua um texto de apresentação. Para já coloque algumas palavras [daqui](https://pl.lipsum.com/). A ideia será ser uma breve apresentação sua que fale por exemplo:
* de motivações para escolher o seu curso, 
* daquilo que mais tem gostado de aprender no curso,
* de espectativas do que gostaria de fazer quando acabar o curso. 
* de hobbies

## 10. Estrutura do Portfolio 

O seu portfolio, no final do semestre, terá uma estrutura como em baixo:

Estrutura da aplicação:
* landing page
* Sobre mim
   * Licenciatura
      * cadeiras 
   * Educação
      * escolas 
   * Aptidões e competências pessoais
      * técnicas
      * organizativas
      * sociais
      * linguisticas
   * Interesses e hobbies
* projetos 
   * realizados por mim
   * projetos de fim de curso interessantes   
* programação Web   
   * tecnologias existentes
      * front-end
      * back-end
      * outras 
   * laboratórios 
   * notícias    
* Blog
* Sobre este website
   * tecnologias usadas
   * padrões usados     
* Contacto
* Rodapé

1. Cada tópico conterá uma lista de vários elementos (por exemplo, licenciatura terá uma lista de cadeiras). A informação de cada item será guardada numa base de dados, sendo esta consultada para criar as listagens. Será igualmente possível, caso se tenha autenticado na plataforma, adicionar atraves de formulários novos items. Aprenderemos a fazer isto tudo nas proximas semanas.
1. Construa em HTML & CSS uma pagina que liste 3 cadeiras com a informação especificada em baixo (para já, podem ter texto inventado). ESse template poderá depois ser aplicado às restantes páginas, a construir mais adiante.
2. Na proxima semana construiremos uma base de dados com tabelas que permitirão armazenar toda a informação associada aos vários tópicos. 
3. Depois, se o utilizador fizer login na aplicação, aparecerá no fim de cada página um formulário onde será possível inserir novos elementos (projetos, tecnologias, etc), e editar existentes.
4. Será o seu portfolio, uma carta de apresentação sua na internet muito valorizada no mundo do trabalho. 
5. Cada página irá listar um conjunto de items (cadeiras, projetos, TFCs) que têmassociados um titulo, imagem, texto e mais alguns atributos. Pense num layout de items independentes / tipo postais, como feito no laboratório anterior lab.5.


## 11. Conteúdos dos vários items

Durante as próximas semanas deverá recolher algum material. Organize-o para já em pastas. Este depois será inserido na base de dados que construirá. Apresentam-se os tipos de conteúdos que deverá congregar para cada item de cada tópico.

* **Educação**
   * educação, listar Formação, com campos curso, local, período logotipo da instituição
      * cursos superior
      * escolas no secundário
      * certificados
   * licenciatura, pagina que apresenta a lista de cadeiras do curso, organizada por semestre e anos. Quando clicada uma cadeira, aparece informação relativamente a: nome, ano, semestre, ECTS, ano letivo frequentado, topicos abordados, ranking de 1 a 5 estrelas (indicando se gostou ou não), professores (da classe Pessoa com campos nome e link para a sua pagina da lusofona e no linkedin), link para página da cadeira (se existir), lista de projetos realizados (classe projeto)
   * Aptidões e competências pessoais (com atributos titulo, descrição curta, lista de projetos (Projeto) realizados onde foi aplicada essa competência caso se aplique, lista de disciplinas (Disciplina) onde foi trabalhada essa competência caso se aplique)
         * [Técnicas]( https://www.e-konomista.pt/competencias-tecnicas/): 
         * linguagens de programação ou tecnologias, relatórios word, apresentações powerpoint, realização de videos, protótipos
         * [Organizativas]( https://www.e-konomista.pt/competencias-de-organizacao/)
         * [Sociais](https://www.e-konomista.pt/aptidoes-e-competencias-sociais)
         * Linguísticas. lista de linguas estrangeiras faladas, com indicação de nível (proficiente, independente ou elementar), e referencia se existir a certificação obtida ou outra explicação (lingua materna, viveu noutro país)
   * interesses (com atributos titulo, descrição, fotografia e link (e.g., clube de fotografia) 
         * outras atividades
         * desporto
         * hobbies
         * voluntariado

* **projetos**
   * realizados por mim: lista de projetos realizados, com atributos: titulo, descrição até 500 carateres, imagem, ano de realização, cadeira (classe Cadeira, caso tenha sido projeto associado a uma cadeira), participantes (da classe Pessoa, da classe Pessoa com atributos nome e link para a sua pagina no linkedin, e link para a aplicação portfolio do projeto PW), link para repositorio GitHub, link para video no youtube, tecnologias usadas, competencias (classe Competencia)
   * trabalhos de fim de curso: lista de 6 Trabalhos finais de Curso (TFCs) de anos passados realizados por colegas seus que achou interessantes, onde TFC tem atributos: titulo, autor (multiplos), orientador (multiplos), ano de realização, sumário, resumo até 500 carateres, link para relatório, links para repositório github e vídeo no Youtube, se existentes. Será facultada uma pasta com relatórios de TFC dos últimos anos para escolher.

* **Programação Web**
   * Tecnologias: Falar das seguintes Tecnologias, com os atributos: nome (por extenso), acrónimo (caso exista, e.g., CSS para Cascade Style Sheet), ano de criação, criador, logotipo, link para site oficial, descrição das principais características. 
         * Back-end: Laravel, ASP.NET, Spring MVC, Express, Django
         * Front-end: Angular, React, Vue, Svelte
         * Outras: WordPress, OutSystems, Weebly, Wix
   * Laboratórios: página que lista links para os laboratórios realizados na disciplina de PW, com o título e descrição dos tópicos abordados
   * Notícias: listagem de 10 noticias sobre artigos do medium.com que tenha gostado, com campos: título, 3 linhas de texto, imagem e link
   * exemplos de técnicas e efeitos que gosta, sites que gosta e de sites que acha maus, tendencias modernas de programação Web, aspectos obsoletos

* **Blog**. Post tem atributos autor, data, título e descrição e eventualmente um link (para projeto, página do seu portfolio) e foto. deverá ter pelo menos 5 posts de outros colegas seus a comentar que gostaram de fazer um determinado projeto consigo, ou de certo trabalho que você fez, ou que é um bom colega para estudar

* **Sobre**, informação sobre este website, incluindo
   * lista de tecnologias usadas na criação do website: HTML, CSS, Python, Django, Heroku, JavaScript). Tecnologia terá os seguintes atributos: nome (por extenso), acrónimo (caso exista, e.g., CSS para Cascade Style Sheet), ano de criação, criador, logotipo, imagem exemplificativa (excerto de código, e.g.) link para site oficial, descrição do que é e onde & como foi usado. 
   * lista de padrões usados: padrão arquitetural cliente-servidor HTTP, padrão de software MVC, padrão de comunicação assíncrona (AJAX) 

* **Contacto**
   * links para a sua conta linkedin. se não tiver, crie. Adicione à sua conta de colegas seus, amigos e professores e adira a grupos de interesse na sua área (DEISI)
   * link para o seu github
   * link para conta Instagram, facebook
   * nome da cidade onde vive
   * facebook, instagram

* **Rodapé**
   * link para Mapa do site
   * contacto
   * nome do autor
   * ano
   * universidade
   * logotipo



 
