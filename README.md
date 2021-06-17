# Descrição das Atividades e Raciocínios
## Lógica Utilizada

A estratégia utilizada foi de executar o Compose e analisar os "logs" que eram gerados, observando os erros que iriam aparecendo, tanto no log da IDE como no browser, e corrigindo em seguida. 

## Etapas do processo
### - Build Compose

O primeiro problema detectado foi com o Compose, o mesmo não estava "buildando". Fui observar os arquivos relacionados como docker-compose.yml e Dockerfile, encontrando erros na criação dos arquivos.

Realizando as devidas alterações, consegui executar o Compose e construir a aplicação com base no arquivo YAML.

Com a aplicação construida observei que os serviços estavam "down", e mais uma vez fui analisar o arquivo Dockerfile e docker-compose para identificar se as portas estavam corretas. Detectei que estavam invertidas, alterei o arquivo docker-compose e o serviço "writer" se tornou "up.

### - Executar os Serviços

Após analise mais geral da aplicação, resolvi executar os serviços independente: Reader, Writer e Redis. Pude notar alguns detalhes:

1. O serviço Reader não estava "up";
2. O serviço Writer estava com falhas, segundo o log;
3. A conexão com o banco Redis estava falhando;
4. A criação do serviço Redis estava incorreta, sendo nomeado de "reidis" ao invés de "redis".

Com isso, decidi focar em cada serviço para torná-lo funcional.

#### > Reader
Com o Reader precisei ajustar o Dockerfile, adicionando os comandos  (RUN) para que os módulos necessários fossem instalados no container. Além disso era necessário a criação do arquivo go.mod. Seguindo a documentação disponibilizada pelo Docker, alterei o comando CMD, trocando por ENTRYPOINT como sugerido. Feito as alterações, foi possível construir e executar a aplicação main.go. 

Uma pequena alteração foi necessária no arquivo main.go, ao construir a aplicação detectei uma falha no log referente a um erro no código. Realizei o reparo e ao construir novamente o container foi criado com sucesso.

#### > Writer
Com o Writer as alterações foram menores. O arquivo requirements.txt foi criado para adicionar o módulo redis ao arquivo main.py. Este foi criado de forma manual e executado no containers por meio do pip. Além disso foi alterado o comando CMD e ENTRYPOINT, mantendo apenas o CMD como sugere a documentação.

#### > Redis
A criação do Redis não teve tanto problema, exceto por um erro na nomeação do serviço, que foi resolvido em um primeiro momento.

#### > Web
O serviço web não sofreu alterações, não vi necessidade de modificar para que o serviço fosse executado.

### - Estabelecendo a Comunicação entre os serviços

Com todos os serviços funcionando de forma individual foi o momento de executar o Compose e analisar se todos estariam executando e interagindo em si. No Home da aplicação foi apresentado que os serviços estavam "up" e ao testar obtive sucesso, visando a aplicação de forma geral.

## Observações, Sugestões e Melhorias

1. Atualização da pagina Reader;
2. Bug encontrado no arquivo main.go;
3. Uso de volumes para os serviços;
4. Organização de Networks no Compose;
5. Atenção para um "warning" ao levantar o serviço reader, referente ao golang.