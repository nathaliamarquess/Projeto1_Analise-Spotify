# Projeto1_Analise-Spotify
Projeto de análise de dados do Spotify utilizando SQL no Google BigQuery e Looker Studio. Realizei limpeza, transformação e análise exploratória dos dados para responder perguntas de negócio, identificar padrões de streams e desenvolver um dashboard com os principais insights.

# Tratamento dos valores nulos na tabela "track_in_competition"

SELECT COUNT (*)
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE track_id IS NULL;

SELECT COUNT (*)
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_apple_playlists IS NULL;

SELECT COUNT (*)
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_apple_charts IS NULL;

SELECT COUNT (*)
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_deezer_playlists IS NULL;

SELECT COUNT (*)
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_deezer_charts IS NULL;

SELECT COUNT (*)
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_shazam_charts IS NULL;

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_shazam_charts IS NULL;

> Encontrado 50 valores nulos na coluna in_shazam_charts de um total de 953 valores.

SELECT COUNT(*) AS quantidade_zero
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_shazam_charts = 0;

> Na coluna in_shazam_charts, existem 344 valores como "0", acredito que "0" signifique que a música não entrou no ranking e que o "null" signifique ausência de informação. Descobri o nome de 3 músicas com valores nulos, na coluna in_shazam_charts, através do id na tabela spotify, depois pesquisei um por um no google e constatei que realmente não entraram no ranking, por isso vou considerar os "null" como "0". 

> Usei o comando CREATE OR REPLACE TABLE para criar uma nova tabela (track_in_competition_tratados) com os nulos tratados; depois usei a cláusula COALESCE para substituir o null por 0, e o REPLACE para não duplicar a coluna in_shazam_charts.

CREATE OR REPLACE TABLE `projeto-1-spotify.desempenho_musical.track_in_competition_tratados` AS 
SELECT *
REPLACE (COALESCE (in_shazam_charts, 0) AS in_shazam_charts)
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`;


# Tratamento dos valores nulos na tabela "track_in_spotify"

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE track_id IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE track_name IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE artists_name IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE artist_count IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE main_music_genre IS NULL;

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE artists_name = "Taylor Swift";


> Encontrado 1 valor nulo na coluna main_music_genre, como a linha é da música Style da Taylor Swift, pesquisei no google o gênero que é pop e decidi alterar "null" por "Pop" (como está escrita a maioria das suas músicas).

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE main_country IS NULL;

> Encontrado 1 valor nulo na coluna main_country, como a linha é da música Style da Taylor Swift, pesquisei no google o país que é Estados Unidos e decidi alterar "null" por "United States" (como está escrito nas suas outras músicas).

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE released_year IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE released_month IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE released_day IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE in_spotify_playlists IS NULL;

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE in_spotify_charts IS NULL;

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE in_spotify_charts IS NULL;

> Identificado 4 valores nulos na coluna in_spotify_charts. Como são poucos os valores nulos encontrados,pesquisei um por um no google e constatei que realmente não entraram no ranking, por isso vou considerar os "null" como "0".

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE streams IS NULL;

> Fazendo o tratamento do nulos: Usei o comando CREATE OR REPLACE TABLE para criar uma nova tabela (track_in_spotify_tratados) com os nulos tratados; depois usei a cláusula COALESCE para substituir os null pelos valores, e o REPLACE para não duplicar as colunas.

CREATE OR REPLACE TABLE `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` AS
SELECT * 
REPLACE (
  COALESCE (main_music_genre, "Pop") AS main_music_genre,
  COALESCE (main_country, "United States") AS main_country,
  COALESCE (in_spotify_charts, 0) AS in_spotify_charts
)
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`;


# Tratamento dos valores duplicados na tabela "track_in_spotify_tratados"

SELECT track_name, artists_name, 
COUNT (*) AS Quantidade
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
GROUP BY track_name, artists_name
HAVING COUNT (*) > 1;

> Foram encontrados 8 grupos duplicados (track_name - artists_name): SNAP - Rosa Linn ; About Damn Time -	Lizzo; Take My Breath	- The Weeknd; SPIT IN MY FACE!	- ThxSoMch; The Astronaut	- Jin; Privileged Rappers	- Drake, 21 Savage; BackOutsideBoyz	- Drake; Broke Boys	- Drake, 21 Savage.

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "SNAP" AND artists_name = "Rosa Linn" ;

> Linhas duplicadas: SNAP - Rosa Linn. Como tem os mesmos valores das colunas mais relevantes, vou excluir a linha que parece faltar dados (track_id = 3814670) e manter a linha que aparentemente tem mais dados, in_spotify_chart preenchido (track_id = 5675634).

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "About Damn Time" AND artists_name = "Lizzo" ;

> Linhas duplicadas: About Damn Time -	Lizzo. Existem alguns valores relevantes diferentes, como o mês e o dia de lançamento, pesquisei no google e verifiquei que a data de lançamento da música foi 14 de Abril de 2022. Portanto, irei excluir a linha com a data errada (track_id = 5080031) e manter a linha com a data correta (track_id = 7173596).

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "Take My Breath" AND artists_name = "The Weeknd" ;

> Linhas duplicadas: Take My Breath	- The Weeknd. Os valores das colunas mais relevantes estão iguais, porém o número de playlists e streams estão diferentes, pesquisei no google pra saber esses valores, não existe nada divulgado, mas encontrei uma informação importante (A música iniciou o ano de 2023 com cerca de 440 milhões de streams acumulados. Ao final de 2023, ela estava se aproximando da marca de 490 milhões.Ou seja, de forma estimada, recebeu cerca de 45 a 50 milhões de streams ao longo de 2023). Portanto, vou considerar o número de streams mais próximo desse valor, assim vou excluir a linha (track_id = 4586215) e manter a linha (track_id = 1119309).

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "SPIT IN MY FACE!" AND artists_name = "ThxSoMch" ;

> Linhas duplicadas: SPIT IN MY FACE!	- ThxSoMch. Os valores das colunas mais relevantes estão iguais, porém com playlists, rankins e streams diferentes, pesquisei no google e constatei que a música não entrou no ranking. Portanto, irei excluir a linha com o ranking 14 (track_id = 4967469) e manter a linha com a o ranking 0 (track_id = 8173823).

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "The Astronaut" AND artists_name = "Jin" ;

> Linhas duplicadas: The Astronaut	- Jin. Todos os campos estão com os mesmos valores, portanto vou excluir a 2ª linha e manter a 1ª linha.

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "Privileged Rappers" AND artists_name = "Drake, 21 Savage" ;

> Linhas duplicadas: Privileged Rappers	- Drake, 21 Savage. Todos os campos estão com os mesmos valores, portanto vou excluir a 2ª linha e manter a 1ª linha.

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "BackOutsideBoyz" AND artists_name = "Drake" ;

> Linhas duplicadas: BackOutsideBoyz	- Drake. Todos os campos estão com os mesmos valores, portanto vou excluir a 2ª linha e manter a 1ª linha.

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_name = "Broke Boys" AND artists_name = "Drake, 21 Savage" ;

> Linhas duplicadas: Broke Boys	- Drake, 21 Savage. Todos os campos estão com os mesmos valores, portanto vou excluir a 2ª linha e manter a 1ª linha.


> Para fazer o tratamento das duplicadas: Usei o comando CREATE OR REPLACE TABLE para atualizar a tabela track_in_competition_tratados com as duplicadas tratadas; depois usei o DISTINCT para 'excluir' da seleção as linhas duplicadas 100% iguais; depois usei o WHERE ... NOT IN, para não incluir as linhas com ose seguintes track_id: 3814670, 5080031, 4586215, 4967469. 

CREATE OR REPLACE TABLE `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` AS
SELECT DISTINCT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` 
WHERE track_id NOT IN (
  3814670, 5080031, 4586215, 4967469
);

# Tratamento dos valores duplicados na tabela "track_in_competition_tratados"

SELECT track_id,
COUNT (*) AS Quantidade
FROM `projeto-1-spotify.desempenho_musical.track_in_competition_tratados` 
GROUP BY (track_id)
HAVING COUNT (*) > 1;

> Não existe nenhuma track_id duplicada.

> Portanto, usando o CREATE OR REPLACE TABLE, WHERE .. NOT IN, vou apenas atualizar a tabela track_in_competition_tratados, excluindo apenas as linhas das track_id que decidi excluir (linhas duplicadas) da tabela track_in_spotify: (track_id = 3814670) , (track_id = 5080031) , (track_id = 4586215) , (track_id = 4967469).

CREATE OR REPLACE TABLE `projeto-1-spotify.desempenho_musical.track_in_competition_tratados` AS
SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_competition_tratados` 
WHERE track_id NOT IN (
      "3814670", "5080031", "4586215", "4967469"  
);

# Tratamento dos valores atípicos em variáveis categóricas na tabela "track_in_spotify_tratados" 

> Procurando na coluna nome da música. Encontrado nenhum.

SELECT track_name, artists_name,
COUNT (*) AS quantidade,
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados`
GROUP BY track_name, artists_name
ORDER BY track_name ASC;

> Procurando na coluna nome do artista. Encontrado nenhum.

SELECT artists_name,
COUNT (*) AS quantidade,
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados`
GROUP BY artists_name
ORDER BY artists_name ASC;

> Procurando na coluna gênero. Encontrei Disco pop e Disco-pop; existe um provável erro de preenchimento ou houve uma falha, pois está preenchido Main genre, não existe esse gênero; As Músicas com gênero preenchido "Main genre" são Something In The Way - Remastered 2021 do Nirvana e Smells Like Teen Spirit - Remastered 2021 do Nirvana, pesquisei no google e o gênero principal dessas músicas é: Rock. Irei substituir Main genre por Rock. E irei também substituir Disco pop por Disco-pop.

SELECT  DISTINCT main_music_genre
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados`
ORDER BY main_music_genre ASC;

SELECT *
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados`
WHERE main_music_genre = "Main genre";

CREATE OR REPLACE TABLE `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` AS
SELECT *
REPLACE (
CASE
WHEN main_music_genre = "Main genre" THEN "Rock"
WHEN main_music_genre = "Disco pop" THEN "Disco-pop"
ELSE main_music_genre
END AS main_music_genre
)
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados`;

> Procurando na coluna país. Encontrei USA e United States, vou substituir USA por Unite States. Encontrei MX e Mexico, vou substituir MX por Mexico. Encontrei PR e Puerto Rico, vou substituir PR por Puerto Rico.

SELECT DISTINCT main_country,
COUNT (*) AS quantidade,
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados`
GROUP BY main_country
ORDER BY main_country ASC;

CREATE OR REPLACE TABLE `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados` AS
SELECT *
REPLACE (
  CASE
  WHEN main_country = 'USA' THEN 'United States'
  WHEN main_country = 'MX' THEN 'Mexico'
  WHEN main_country = 'PR' THEN 'Puerto Rico'
  ELSE main_country
  END AS main_country
)
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify_tratados`;
