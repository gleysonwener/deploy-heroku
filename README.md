# deploy-heroku
Passo a passo para deploy de uma aplicação no Heroku

Passo a passo para “deployar” aplicações no Heroku

	Criar conta no heroku
	Baixar heroku Cli
	Instalar heroku cli
#lembrar de ativar a virtualenv e estar no diretório raiz do projeto
	Heroku –version verifica a versão e se foi instalado o heroku cli

No terminal:
	Digite: heroku login
#Ele vai pedir os dados eu foram cadastrados na hora da criação da conta, depois ele vai dar a mensagem de sucesso de login
	git init
	Criar arquivo .gitignore na raiz do projeto e adicionar as pastas e arquivos necessários(tipo: venv, .idea)
	git add .
	git commit –m “descrição do commit”

Fazendo leitura de variáveis de ambiente:
	pip install python-decouple
	criar arquivo  .env na raiz do projeto

Digitar dentro do arquivo .env:
	SECRET_KEY=valor do seu SECRET_KEY
	DEBUG=False

Dentro do arquivo settings.py digite:
	from decouple import config

Altere a SECRET_KEY para: 
	SECRET_KEY = config(‘SECRET_KEY’)

Altere o DEBUG para:
	DEBUG = config(‘DEBUG’, default=False, cast=bool)

Configurar Banco de dados:
	pip install dj-database-url

Importar no settings.py:
	from dj_database_url import parse as dburl

Substituir o database lá do settings.py:
	default_dburl = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')
DATABASES = { 'default': config('DATABASE_URL', default=default_dburl,
cast=dburl), }

Importar no settings.py
	import os
# aqui o heroku vai utilizar o banco de dados padrão que é o Postgresql

Arquivo estáticos, digite no terminal:
	pip install dj-static

Dentro do aquivo wsgi.py no diretório do projeto
	importar: from dj_static import Cling
	application = Cling(get_wsgi_application())
#verifica todas as requisições que chegar, senão for estático ele manda pro Django

No settings.py definir: 
	STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

No terminal digitar:
	pip freeze > requirements-dev.txt
#exporta todas bibliotecas instalada para funcionamento da aplicação

Criar o arquivo requirements.txt e adicionar os arquivos e digitar o seguinte:
	-r requirements-dev.txt
	gunicorn
	psycopg2
# aqui o heroku vai ler esse arquivo e instalar todas as bibliotecas necessárias para o funcionamento da aplicação que vai subir pra internet

Criar um arquivo com o nome Procfile na raiz do projeto e adicionar:
	web: gunicorn project.wsgi --log-file -
#onde o project é o nome do seu projeto onde está o arquivo wsgi
	#exemplo: web: gunicorn loja1.wsgi --log-file -

Criar arquivo runtime.txt e adicionar o ambiente desejado, digite:
	python-3.10.4
# aqui diz pro heroku qual o ambiente que nós vamos querer, que é a o python que está instalado na sua máquina e sua versão

Criar app no heroku:
	heroku apps:create app-name
#app-name é o nome da sua aplicação
#você pode também criar no próprio site do heroku
#o heroku já vai disponibilizar um domínio pra sua aplicação

Inserir o domínio do heroku no ALLOWED_HOSTS no settings.py:
	ALLOWED_HOSTS = [‘nome do domínio no heroku sem o https:// e a barra do final’, ‘localhost’]
	Exemplo: ALLOWED_HOSTS = [‘loja1.herokuapp.com, ‘localhost’, ‘127.0.0.1’]
# só funciona se o DEBUG=False

Instalar o plugin heroku-config:
	heroku plugins:install heroku-config

Enviar as configurações do arquivo .env para o heroku:
	heroku plugins:install heroku-config
	heroku config:push

Checar se as configurações das variáveis de ambientes foram para o heroku:
	heroku config

Publicando a aplicação :
	git add .
	git commit -m “descrição do commit”         # aspas duplas mesmo
	git push heroku master

Criar o banco de dados da aplicação no heroku:
	heroku run python manage.py migrate

Criar um super usuário:
	heroku run python manage.py createsuperuser
#adicione as informações

Criado por:
Gleyson Wener
https://github.com/gleysonwener
https://www.linkedin.com/in/gleyson-wener-208b46230/
gleysonwener3@gmail.com


