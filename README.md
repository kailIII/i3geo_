## 1.Tutorial de instalação e configuração

Este tutorial tem como finalidade demonstrar a instalação e configuração do i3geo na distribuição Linux Ubuntu 9.10 e 10.04. Para instalar e configurar o i3geo, copie a pasta I3geo ( [http://mapas.mma.gov.br/download](http://mapas.mma.gov.br/download) ou do SVN do Portal) para o diretório do apache (padrão do Ubuntu <i>/var/www/</i>) e configure os endereços da aplicação conforme instruções dos subitens deste tutorial.

## 2.Configurando o arquivo ms_configura.php

Abra o arquivo <i>/i3geo/ms_configura.php</i> em um editor para configurar as variáveis que guardam os caminhos da aplicação conforme segue abaixo:

* Variável <b>$mensagemInicia</b>: esta variável é responsável pela mensagem inicial do I3Geo, você pode colocar uma mensagem personalizada alterando seu texto conforme exemplo:

        $mensagemInicia = “minha aplicação” ;

* Variável <b>$tituloInstituicao</b>: esta variável é responsável pelo título que aparecerá na barra de título dos navegadores.

        $tituloInstituicao = "Meu Título";
    
<b>Nota:</b> Se for criar mais de uma aplicação usando o i3geo, comente a variável <b>$tituloInstituicao</b> e coloque o título da aplicação na tag title do html.
    
O primeiro bloco de código diz respeito a instalação no windows e o segundo é responsável pela configuração no Linux. É este último bloco de código que iremos alterar conforme indica os procedimentos abaixo:

* Variável <b>$editores</b>: responsável por controlar quais usuários poderão utilizar o aplicativo de administração do i3geo a partir dos IPs das máquinas destes. Desta forma, esta variável deve ser preenchida conforme exemplo abaixo:

        $editores = array("10.0.0.2",”192.23.255.255”);

<b>Obs:</b> No exemplo acima somente as máquinas 10.0.0.2 e 192.23.255.255 terão permissão para editar os temas do i3geo.

* Variável <b>$dir_tmp</b>: responsável por guardar o caminho absoluto do diretório temporário onde serão gravadas as imagens geradas pelo mapserver.

        $dir_tmp = "/tmp/";

<b>Obs:</b> Recomenda-se utilizar o diretório <i>/tmp</i> do Ubuntu, pois este é apagado regularmente pelo próprio sistema operacional, poupando o trabalho do usuário em ter de esvaziar este diretório de tempos em tempos, porém pode-se estabelecer qualquer diretório como sendo o <b>$dir_tmp</b>.

<b>Obs:</b> Certifique-se que este diretório esteja visível pelo <b>Apache2</b> e que este tenha permissão de escrita no diretório.

<b>Obs:</b> Recomenda-se a criação de um link simbólico na pasta <i>/var/www/</i> direcionado para diretório temporário. Para criar um link simbólico digite o seguinte comando no terminal do Ubuntu:

    /var/www:~$ sudo ln - s /tmp/ mp ms_tmp

<b>Nota:</b> com o comando acima criaremos um link simbólico chamado <i>ms_tmp</i> que remete-se ao diretório <i>/tmp</i> do Ubuntu.

* Variável <b>$temasdir</b>: responsável por guardar o caminho absoluto da pasta onde ficam os arquivos mapfiles das camadas que serão visualizadas no i3geo.

        $temasdir = "/var/www/i3geo/temas";

* Variável <b>$temasaplic</b>: responsável por guardar o caminho absoluto da pasta onde ficam os arquivos html das interfaces do i3geo e onde estão os mapfiles de inicialização do aplicativo.
    
        $temasaplic = "/var/www/i3geo/aplicmap";
    
* Variável <b>$locmapserv</b>: indica ao apache o caminho relativo do mapserver cgi.

        $locmapserv = "/cgi-bin/mapserv";

<b>Obs:</b> o mapserver cgi no Ubuntu fica no diretório <i>/usr/lib/cgi-bin/mapserv</i>, crie um link simbólico apontando para este diretório, conforme exemplo abaixo: 

    /var/www/:~$ sudo ln -s /usr/lib/cgi-bin/mapserv cgi-bin

* Variável <b>$locaplic</b>: esta variável é responsável por guardar o caminho absoluto da pasta raiz do i3geo.

        $locaplic = "/var/www/i3geo";

* Variável <b>$R_path</b>: esta variável indica o path do pacote estatístico R, para que o i3geo possa acioná-lo a partir de suas ferramentas de gráficos e análise espacial

        $R_path = "R";

<b>Obs:</b> se você não instalou o R, deixe essa variável vazia.

A partir da versão 4.1 do i3geo, os temas (variável <b>$menutemas</b>), os sistemas (variável <b>$locsistemas</b>), os sistemas que aparecem na ferramenta identifica (variável <b>$locidentifica</b>), os mapas da guia Links (<b>$locmapas</b>) e os atlas (variável <b>$mapasxml</b>) deixaram de ser configuradas por um arquivo <i>xml</i> e passaram a integrar o banco de dados sqlite do sistema de administração, desta forma, estas variáveis devem ficar vazias, conforme demonstrado abaixo:

    $locsistemas= "";
    $locidentifica = "";
    $locmapas = "";
    $menutemas = "";
    $atlasxml = "";

* Variáveis <b>$postgis_con</b> e <b>$srid_area</b>: foram depreciadas e não são mais necessárias a partir da versão 5.x do MapServer.

* Variável <b>$utilizacgi</b>: recebe os valores “sim” ou “não”, em caso afirmativo o i3geo utilizará o cgi do mapserver para gerar os mapas tornando a aplicação mais rápida, em caso negativo as imagens serão geradas pelo php.

* Variável <b>$postgis_mapa</b>: tem a função de esconder a string de conexão com o banco de dados no mapfile. Ao optar por esta funcionalidade do i3geo especifique a sua conexão com o banco na variável, conforme exemplo abaixo. Se você não for usar esta opção deixe a variável em branco.

        $postgis_mapa = array("exemplo"=>"user=postgres password=postgres
        dbname=pgutf8 host=localhost port=5432 options='-c client_encoding=LATIN1'");
    
<b>Obs:</b> Optando por esta funcionalidade o mapa será forçado a recusar o modo de operação CGI.

* Variável <b>$expoeMapfile</b>: recebe os valores “sim” e “não”, e controla se o nome do mapfile atual será ou não retornado para a aplicação via ajax. Quando essa variável for definida como "nao" algumas das funcionalidades do i3geo poderão ficar prejudicadas, mas sem comprometimento das funções principais.

        $expoeMapfile = "sim";

* Variável <b>$conexaoadmin</b>: define a string de conexão com o banco de dados administrativo, originalmente montado no banco de dados sqlite. Para usar o banco default do i3geo deixe esta variável em branco, caso queira utilizar a ferramenta de administração em outro banco de dados determine a string de conexão com o outro banco nesta variável.

* Variável <b>$interfacePadrao</b>: define o arquivo htm, html ou phtml que será usado pelo i3geo para visualização dos mapas. Este aquivo deve estar armazenado na pasta <i>i3geo/aplicmapa</i>.

        $interfacePadrao = "geral.htm";

## 3. Configurando o arquivo geral1.map

Abra o arquivo <i>/i3geo/aplicmap/geral1.map</i> em um editor e configure os caminhos no mapfile conforme demonstrado abaixo: 

* Tag <b>FONTSET</b>: fornece para o mapserver o caminho para o diretório onde está o arquivo de fontes que será utilizado pelo i3geo.

        FONTSET "/var/www/i3geo/symbols/fontes.txt"

* Tag <b>SYMBOLSET</b>: fornece para o mapserver o caminho para o diretório onde está o arquivo de símbolos que será utilizado pelo i3geo.

        SYMBOLSET "/var/www/i3geo/symbols/simbolos.sym"

* Tag <b>SHAPEPATH</b>: fornece para o mapserver o caminho para o diretório onde ficarão armazenados os arquivos cartográficos (shapefiles) que serão utilizados pelo i3geo.
    
        SHAPEPATH "/var/www/geodados"

* Tag <b>IMAGE</b>: fornece para o mapserver o caminho para o diretório onde está a figura usada como mapa de referência.
    
        IMAGE "/var/www/i3geo/imagens/referencia1.png"

* Tag <b>IMAGEPATH</b>: fornece para o mapserver o caminho para o diretório onde serão armazenados as imagens geradas pelo i3geo.
  
        IMAGEPATH "/tmp/"

* Tag <b>IMAGEURL</b>: fornece para o mapserver o caminho relativo para o diretórion temporário. Recomenda-se que a IMAGEURL seja um link simbólico para a IMAGEPATH.
  
        IMAGEURL "/ms_tmp/"

* Tag vTEMPLATE</b>: fornece para o mapserver o caminho para o diretório onde está o arquivo da interface do i3geo (template).
    
        TEMPLATE "/var/www/i3geo/aplicmap/geral.htm"


Configure a Tag <b>DATA<b> de todas as <b>LAYERS<b> do arquivo <i>geral1.map</i> e também do arquivo <i>estadosl.map</i> que se encontra na mesma pasta, conforme o exemplo abaixo:

    LAYER
    DATA "/var/www/i3geo/aplicmap/dados/zee"
    
## 4.Teste de instalação

Para ter certeza se a instalação está correta vá para o navegador de internet e na barra de endereços digite <b>http://<host>/i3geo/testainstal.php</b>.

Este programa fará a verificação se todos os pacotes necessários para o funcionamento do i3geo foram instalados e se os caminhos definidos no <i><b>ms_configura.php</b></i> estão corretos. Além destas verificações listadas acima, o programa <i><b>testainstal.php</b></i> verifica se o apache consegue escrever na pasta temporária e testa os mapfiles <i><b>geral1.map</b></i> e <i><b>estadosl.map</b></i>.

<b>Atenção:</b> Se ao final da listagem de verificação aparecer dois mapas do Brasil na América do Sul, um sem os limites estaduais e outro com os limites, sua instalação está correta e você pode começar a usar o i3geo. Caso estes mapas não apareçam verifique as mensagens de erro e tente corrigi-las para utilizar o i3geo.

