# Trabalho Prático de Arquitetura de Software - Analisando Arquiteturas de Sistemas de Software Populares
Trabalho realizado para a disciplina Arquitetura de Software na instituição Pontifícia Universidade Católica de Minas Gerais - PUC Minas - 2° Semestre 2023

## Integrantes:
- Belle Nerissa
- Richbert Oliveira
- Sara Iglesias

# Sistema Escolhido: Uber

## Motivação


## Uber 

``Uber é o aplicativo líder global de compartilhamento de viagens.``

"A missão da Uber é um transporte tão confiável quanto água corrente, em qualquer lugar, para todos. Para tornar isso possível, criamos e trabalhamos com dados complexos. Em seguida, agrupamos tudo perfeitamente como uma plataforma que permite aos motoristas fazer negócios e aos passageiros se deslocarem."

Em suma, a Uber hoje atua no mercado de transporte de passageiros, oferecendo uma alternativa conveniente e flexível aos táxis tradicionais. Seu nicho de mercado é composto por pessoas que desejam solicitar transporte privado de forma rápida e eficiente por meio de um aplicativo móvel. Seu nicho de mercado é composto por pessoas conectadas a seus celulares móveis que desejam solicitar transporte privado de forma rápida e eficiente por meio de um aplicativo móvel. 

#### Características do produto

![image](https://github.com/Deathpark/Arquitetura-Trabalho-Pratico/assets/49173787/9b682024-24d2-41f2-9d6f-1c50b19c1986) 
1- Reservas brutas e viagens são do ano fiscal de 2022. MAPCs, motoristas, entregadores e comerciantes são do quarto trimestre de 2022.
2- Com base na nossa definição interna de cidade, que inclui áreas metropolitanas compostas por diversas cidades.
3- Como porcentagem das reservas brutas.
4- Baseado em moeda constante.
5- Consumidores ativos mensais da plataforma.

#### O crescimento exponencial dos negócios da Uber

"A Uber começou em 2010 e completou o primeiro bilhão de viagens no final de 2015 . Pouco tempo depois, em meados de 2018 , esse número já tinha subido para 10 mil milhões de viagens. Essas viagens aconteceram em mais de 21 países em 5 continentes. As linhas de negócios da Uber também se expandiram significativamente e o produto tornou-se mais sofisticado. Isso resultou em um crescimento exponencial dos dados registrados, juntamente com uma demanda por capacidade computacional para analisar esses dados.

Embora queiramos que a interface do Uber seja simples, projetamos sistemas complexos por trás dela para permanecerem funcionando, lidar com interações difíceis e atender grandes quantidades de tráfego. Dividimos a arquitetura monolítica original em muitas partes para escalar com o crescimento. Com centenas de microsserviços que dependem uns dos outros, desenhar um diagrama de como o Uber funciona neste momento é extremamente complicado e tudo muda rapidamente. O que podemos abordar em um artigo de duas partes é a pilha que usamos na primavera de 2016."

#### Desafios da Uber
(...) Os principais desafios e oportunidades na redução do custo do big data do Uber, incluem: **armazenamento, escala, recuperação e utilização**. Através da análise, percebemos que esses problemas estavam todos inter-relacionados e que otimizar demais para um poderia prejudicar os outros, por isso desenvolvemos uma estrutura estratégica holística para melhorar a eficiência de custos do nosso big data, com 3 pilares: fornecimento, eficiência da plataforma e demanda.

#### Requisitos de segurança

>“A segurança nunca deve ser proprietária e é nossa intenção causar um impacto muito além da nossa própria empresa, incentivando outros a serem mais transparentes com os seus dados e a partilharem as melhores práticas que podem tornar todos mais seguros.”
<cite>Tony West, diretor jurídico </cite>

##### U-Check: Verificação de parceiros e usuários
A Uber conta com diferentes modalidades de verificação de parceiros e usuários para proporcionar uma experiência tranquila para todos. Todos os parceiros passam por um processo de cadastro de várias etapas antes de aceitar a primeira viagem que envolve checagem de apontamentos criminais e análise da CNH. De tempos em tempos, o app pede, aleatoriamente, para que eles tirem uma selfie antes de aceitar uma viagem ou de ficar on-line para verificação de sua identidade. Os usuários são verificados via cartão de crédito ou têm seus dados de CPF e data de nascimento checados em uma base do governo. Além disso, pode ser necessário também apresentar uma foto do documento de identidade.
###### U-Ajuda: checagem de rota
Este recurso identifica paradas longas inesperadas, mudanças de rota e viagens encerradas antes do previsto. Ele automaticamente manda uma mensagem para o motorista e o usuário perguntando se algum suporte é necessário, direcionando-os para as ferramentas de segurança disponíveis. O recurso utiliza o poder do GPS e de outros sensores no smartphone para identificar essas situações.
##### U-Áudio: Gravação de áudio
Motoristas parceiros e usuários podem utilizar o próprio aplicativo para gravar o áudio da viagem caso não se sintam confortáveis. O conteúdo pode ser enviado para Uber de forma criptografada por meio de um relato de segurança e, em caso de necessidade, utilizado para investigações ou compartilhado com autoridades, na forma da lei. Saiba como gravar suas viagens clicando no botão abaixo.
##### U-Acompanha: compartilhamento de viagem
Tanto motoristas parceiros quanto usuários podem compartilhar a viagem em tempo real com quem desejarem. A ferramenta é acionada por meio da Central de Segurança, identificada por um escudo azul sobre o mapa da viagem no aplicativo.


##### Fontes: 
- Uber-2023-Environmental-Social-and-Governance-Report 
- https://investor.uber.com/home/default.aspx
- https://www.uber.com/en-BR/blog/engineering

##### Fontes: 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d2d5859-0aca-488c-936d-671b5c22fc76/a9f9c34f-bb0e-4d44-91ca-09dbcb476855/Untitled.png)

Estabelecer conexão entre motorista e passageiro parece uma tarefa simples, mas não é.

Até 2014, o Uber utilizava de uma estrutura monolítica mas com sua expansão, foi necessário transicionar para uma arquitetura orientada a serviços. Além da necessidade de uma aplicação mais escalável, essa mudança para a arquitetura de microsserviços foi impulsionada pela necessidade de um gerenciamento mais específico de viagens, dados do passageiro e do motorista, além das cobranças, pagamentos e disparo de notificações.

A arquitetura da Uber se divide em dois sistemas que se comunicam com um terceiro que faz uma ponte entre os dois. Os dois primeiros são o **supply** (para motoristas) e o **demand** (para passageiros), e ambos se comunicam com o ****dispatch (DISCO). O dispatch é responsável pelo mapa e pela localização, utilizando-se do Google S2 Geometry Library para buscar dados de latitude e longitude de forma tridimensional ao invés de planificada, como se fosse um atlas. Esse recurso possibilita o cálculo do raio e dispara a notificação para os motoristas que se encontram dentro da localização especificada e dessa forma é possível entre o motorista e o passageiro.

A cada 4 segundos, é enviada a localização dos motoristas para a REST API do Kafka, que envia essa informação de localização para o dispatch e ambas as camadas de supply e demand tem acesso à essas localizações.

Quando o usuário vai pedir um Uber, essa requisição passa primeiro pelo WebSocket. O WebSocket faz essa requisição para o demand, que se comunica com o supply com as informações da viagem (tipo de corrida, quantas corridas são necessárias, localização). O serviço de supply, então, é responsável por localizar os motoristas que estão mais próximos contanto também com o cálculo do ETA (tempo estimado de chegada) e depois que esse valor é calculado, o supply notifica aos motoristas se eles aceitam determinada corrida.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d2d5859-0aca-488c-936d-671b5c22fc76/7c3be887-69d5-4767-8c1e-095fe70ffc99/Untitled.png)

Na imagem acima, podemos ver que existem vários servidores que guardam as geolocalizações. Se alguma outra localização for adicionada, o próprio sistema já sabe dos servidores disponíveis e distribui as células de localização. Se um servidor for removido, as células são redistribuídas entre os servidores existentes.

As bases de dados são NoSQL, sendo elas horizontalmente escaláveis e possuem alta disponibilidade visto que a localização dos motoristas é enviada a cada 4 segundos para mudança de estado.

Quanto à questão da segurança, a Uber lida com dados em tempo real, ou seja, são necessários sistemas internos e uma equipe de UX e ciência de dados para monitorar fraudes e eventos indesejados.

Referências:

https://www.infoq.com/br/presentations/a-arquitetura-de-sistemas-de-tempo-real-da-uber/

https://www.geeksforgeeks.org/system-design-of-uber-app-uber-system-architecture/

https://medium.com/nerd-for-tech/uber-architecture-and-system-design-e8ac26690dfc

