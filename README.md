# Projeto1_Analise-Spotify
Projeto de análise de dados do Spotify utilizando SQL no Google BigQuery e Looker Studio. Realizei limpeza, transformação e análise exploratória dos dados para responder perguntas de negócio, identificar padrões de streams e desenvolver um dashboard com os principais insights.

-- Identificando valores nulos na tabela "track_in_competition"

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

-- Encontrado 50 valores nulos na coluna in_shazam_charts de um total de 953 valores.

SELECT COUNT(*) AS quantidade_zero
FROM `projeto-1-spotify.desempenho_musical.track_in_competition`
WHERE in_shazam_charts = 0;

-- Na coluna in_shazam_charts, existem 344 valores como "0", acredito que "0" signifique que a música não entrou no ranking e que o "null" signifique ausência de informação. Descobri o nome de 3 músicas com valores nulos, na coluna in_shazam_charts, através do id na tabela spotify, depois pesquisei um por um no google e constatei que realmente não entraram no ranking, por isso vou considerar os "null" como "0".

-- Identificando valores nulos na tabela "track_in_spotify"

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


-- Encontrado 1 valor nulo na coluna main_music_genre, como a linha é da música Style da Taylor Swift, pesquisei no google o gênero que é pop e decidi alterar "null" por "Pop" (como está escrita a maioria das suas músicas).

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE main_country IS NULL;

-- Encontrado 1 valor nulo na coluna main_country, como a linha é da música Style da Taylor Swift, pesquisei no google o país que é Estados Unidos e decidi alterar "null" por "United States" (como está escrito nas suas outras músicas).


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

-- Identificado 4 valores nulos na coluna in_spotify_charts. Como são poucos os valores nulos encontrados,pesquisei um por um no google e constatei que realmente não entraram no ranking, por isso vou considerar os "null" como "0".

SELECT COUNT (*) 
FROM `projeto-1-spotify.desempenho_musical.track_in_spotify`
WHERE streams IS NULL;




