# GEOCODING & MAPS
vamos falar então de geocoding.

# GEOCODING
o que o geooding faz?
ele transforma endereços em coordenadas GPS. (Address -> GPS Coordinates)
geocoding is a gem então vamos instalá-la.

# START WITH THE RAILS MINIMAL TEMPLATE
hoje eu ja fiz o set up inicial aqui, ja fiz tambem  o do push pro heroku pq ele é bastante demorado.
No demo de hoje nos vamos utilizar o minimal template do wagon porque nos não vamos precisar de uma tabela usuarios.
ENTÃO O QUE EU FIZ:
Ja criei meu app, ja mandei pro github e ja fiz tambem o push pro heroku. Tudo bem? so pra
gente ganhar um tempinho.

# A NEW GEM TO YOUR GEMFILE
goecoder é um gem, então pra isso precisamos colocá-la no gemfile, fazer o bundle install
e fazer também algumas instalações e configurações específicas do proprio geocoding.
ele vai criar pra gente o arquivo geocoder.rb - um arquivo de config que tb vem todo comentado
cabe ao dev descomentar as linhas que ele precisa utilizar.

# SOME CONFIGURATION FIRST

de todas essas que estão aqui, existem duas que vamos dar uma olhada, e se tiverem curisosidae
em relação às outras, basta dar uma olhadinha na documentação.

a primeira qur vamos olhar é a ***lookup*** -> lookup nos diz qual é a ferramenta que ele vai 
utilizar para transformar o endereço em latitude e longitude e vice-versa - no caso aqui ele vai usar o `:nominatim`, que é o padrão, mas aqui na documentação fala que você pode escolher outros configs, mas que no caso seriam pagas. O QUE O NOMINATIM FAZ?
Nominatim usa dados do OpenStreetMap para encontrar locais na Terra por nome e endereço, ou seja, geocodificação. Ele também pode fazer o inverso, ele pode encontrar um endereço para qualquer local do planeta a partir de coordenadas.

***frances***
Alguemm conhece o SM? vamos dar uma ohadinha rapida aqui:
O OpenStreetMap fornece dados de mapas para milhares de sites, aplicativos e dispositivos móveis.
É construído por uma comunidade de cartógrafos voluntários que contribuem e mantêm dados para estradas, trilhas, cafés, estações de trem e muito mais, em todo o mundo.
***frances***

Outro config que é super importante pra gente é o ***units** -> nos diz qual a unidade de medida que ele vai utilizar. Vamos então trocar aqui para km, certo? A não ser que alguém queira milhas mesmo, e tá tudo bem!

Lembrando que esses arquivos de config, assim como o devise.rb, geocoder.rb ele rodam UMA VEZ quando o rails está iniciando.

temos tambem a API, é aqui que armazenamos a chave de API que vamos utilizar. exemplo, se eu quiser 
utilizar o google maps API, vou la no meu .env, pego a variavel GOOGLE_API_KEY (ou seja la qual for seu
valor), e vou passa-la aqui como env, ok? somente a variavel, não seu valor, ok?
`api_key: ENV[MAPBOX_API_KEY],`

# SUPPOSE WE ALREADY HAVE A FLAT - suponhamos que - temos um flat já aqui:
Vamos fazer um scaffold de um flat... 
Antigamente era muito complicado criar feeds de dados de JSON adequados. Felizmente, Ruby on Rails torna muito mais fácil lidar com JSON. e o gem JBuilder, nos permite construir facilmente feeds de dados complexos no formato JSON.

https://api.mapbox.com/geocoding/v5/mapbox.places/consola%C3%A7%C3%A3o.json?access_token=pk.eyJ1IjoidGF0Y2hpd2lnZ2VycyIsImEiOiJja2VsaGgwYjQwMHMxMnJydHhmenhwbzA3In0.3v-IIKKLq76oFIndWDspnA

Então vamos gerar um Flat com nome e endereço e o endereço vai ser o que vamos usar para gerar a
latitude e a longitude.

# ADD COLUMNS TO YOUR MODEL
Criamos nosso flat, vamos fazer um
RAIS DB:MIGRATE e agora precisamos adicionar duas colunas no nosso modelo - uma de lat e uma de long.

# GEOCODING
Vocês vão perceber que existe muita configuração no geocoder, mto copy paste mesmmo, e poucas coisas
pra vocês personalizarem.
Vamos adicionar essas duas linhas aqui no modelo Flat, o que elas fazem:

`geocoded_by :addres's` -> o geocoder vai trabalhar em cima do campo address, e daqui que ele vai tirar
a lat e long;
`after_validation :geocode, if: :will_save_change_to_address?` -> aqui nos falamos pra ele rodar o geocoder de novo caso os dados do endereço sejam alterados. Então ele checa se depois que o campos address foram validados, o endereço continua o mesmo, se continua, td ok, senão ele roda o geocoder de novo e atualiza a longitude e latitude.

# RESULT
VAMOS TESTAR NO RAILS C
- CRIAR UM FLAT
- "Rua Matias Aires 440, Consolação"
- colar lat e long no google
- fazer update etc

# SEARCH
a gente tambem consegue pegar uma região proxima de uma área qualquer utilizando o comando near.
Entao por exemplo o carlos quer me visitar em SP e quer ver o que tem aqui perto da minha casa
ele passa Flat.near(tatchi, e como escolhemos a unidade de medida a gente passa ela aqui.) ele pegou aqui todos os Flats que são proximos ao meu endereço em um raio de 5km.

Flat.near('Tour Eiffel', 10)
Flat.near('Cambuci São Paulo', 10)
Flat.near('Belo Horizonte', 10)

# MAPBOX MAPS
Então mapbox é uma api que nos permite utilizar mapas na nossa aplicação e tambem nos permite
adicionar markers no nosso mapa.

# WE NEED TO SET UP AN API KEY FOR MAPBOX
# Add this key to your .env.
# SECURITY

Ja adicionamos a API KEY no nosso .env porque a gente precisva dela pra fazer a atualização dos 
endereços funcionar. Mas vamos então no nosso controller que ja foi gerado para alterar o codigo
la no nosso index.

# CONTROLLER
copy paste do arquivo - go to controller to explain

# VIEW - STIMULUS
<div style="width: 100%; height: 600px;"
  data-controller="mapbox"
  data-mapbox-markers-value="<%= @markers.to_json %>"
  data-mapbox-api-key-value="<%= ENV['MAPBOX_API_KEY'] %>"></div>
uma outra coisa que vamos precisar fazer tambem e alterar nosso view
<!-- app/views/flats/index.html.erb -->

# STIMULUS CONTROLLER

# IMPORT THE STYLE

# STIMULUS OOP
- export default class extends Controller define uma classe JavaScript;
- this.variableName refere-se a uma variável de instância, disponível em qualquer lugar dentro da classe
- this.element refere-se à variável de instância do elemento HTML do controlador do stimulus

# ADD MARKERS
beleza temos um mapa - fizemos somente o mapbox. mas ta faltando nossos markers ne?
então vamos nos slides e adicionar o codigo no nosso controller e depois de criar o mapa.

# FIT MAP TO MARKERS
bom, nos temos um mapa inteiro e a genre precisa fazer um zoom pra pode chegar nos markers
que queremos. Existe uma solução pra isso - sim. Aqui é que entra o fit map to markers; 
Então a proxima coisa que vamos adicionar no nosso nosso é isso aqui.

# PRODUCTION
joga pro heroku
add flats
mamae = Flat.create!(address: 'rua copernico 170, Belo Horizonte')
tatchi = Flat.create!(address: 'Rua Matias Aires 440, Consolação')

# GOING FURTHER

# INFO WINDOWS (1)
Primeiro, vamos atualizar nosso controller Rails para passar mais informações sobre nossos flats para nosso view:
por enquanto eu so quero que apareça o nome e o endereço;
aqui eu sei o que é flat, porque quando esse arquivo for renderizado, eu passo o que é flat no meu flats controller dentro de => locals: { flat: flat }

 # ADD A PARTIAL FOR OUR INFO WINDOW

 # ADD THE INFO WINDOW TO EACH MARKER

# ADD SOME STYLING TO OUR POPUPS

 # CUSTOM MARKERS
 existem 2 formas de vcs customizarem os popups de vcs.. vcs podem guardar uma imagem
 nos assets de passa-la aqui como como argumento da asset_url
 image_url: helpers.asset_url('REPLACE_THIS_WITH_YOUR_IMAGE_IN_ASSETS')

 # CUSTOM MARKERS (2)
 ou vocês podem utilizar esse codigo aqui, colocar la dentro do add markersr to map

 # SEARCHING ON YOUR MAP

# STYLING YOUR MAP

 # ADDRESS INPUT AUTOCOMPLETE
 ADICIONA O AUTOCOMPLETE AO CRIAR UM FLAT

# CREATE THE STIMULUS CONTROLLER

# UPDATE THE RESULTS

# STYLE THE SEARCH INPUT