# README
a) NIVEAU FACILE

Quel est le nombre total d'objets Album contenus dans la base (sans regarder les id bien sûr) ?
Album.count
  Album Count (0.2ms)  SELECT COUNT(*) FROM "albums"
=> 347

Qui est l'auteur de la chanson "White Room" ?
track = Track.find_by(title: "White Room")
artist = track.artist
=> "Eric Clapton"

Quelle chanson dure exactement 188133 milliseconds ?
Track.find_by(duration: 188133)
  Track Load (0.3ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."duration" = ? LIMIT ?  [["duration", 188133], ["LIMIT", 1]]
=> 
#<Track:0x000000011b50e8a0
...
irb(main):008> name = track.title
=> "Perfect"
irb(main):009> artist = track.artist
=> "Alanis Morissette"

Quel groupe a sorti l'album "Use Your Illusion II" ?
 Album.find_by(title: "Use Your Illusion II")
  Album Load (0.2ms)  SELECT "albums".* FROM "albums" WHERE "albums"."title" = ? LIMIT ?  [["title", "Use Your Illusion II"], ["LIMIT", 1]]
=> 
#<Album:0x000000011ace1e20
...
irb(main):011> artist = album.artist
=> "Guns N Roses"

NIVEAU MOYEN
Combien y a t'il d'albums dont le titre contient "Great" ? 
Album.where("title LIKE ?", "%Great%").count
  Album Count (20.8ms)  SELECT COUNT(*) FROM "albums" WHERE (title LIKE '%Great%')
=> 13

Album.where("title LIKE ?", "%Great%")
  Album Load (0.3ms)  SELECT "albums".* FROM "albums" WHERE (title LIKE '%Great%') /* loading for pp */ LIMIT ?  [["LIMIT", 11]]
=> 
[#<Album:0x000000011ad03c00
...
irb(main):017> tp albums, :title, :artist
  Album Load (0.2ms)  SELECT "albums".* FROM "albums" WHERE (title LIKE '%Great%')
TITLE                          | ARTIST                        
-------------------------------|-------------------------------
Greatest Hits II               | Queen                         
Greatest Kiss                  | Kiss                          
Vault: Def Leppards Greates... | Def Leppard                   
Greatest Hits                  | Lenny Kravitz                 
Motley Crue Greatest Hits      | Mötley Crüe                   
Greatest Hits I                | Queen                         
Rotten Apples: Greatest Hits   | Smashing Pumpkins             
The Police Greatest Hits       | The Police                    
Great Opera Choruses           | Chicago Symphony Chorus, Ch...
Great Performances - Barber... | Leonard Bernstein & New Yor...
Great Recordings of the Cen... | Gustav Mahler                 
Great Recordings of the Cen... | Itzhak Perlman                
Great Recordings of the Cen... | Gerald Moore     


Supprime tous les albums dont le nom contient "music".
albums_to_delete = Album.where("title LIKE ?", "%music%")
SELECT "albums".* FROM "albums" WHERE (title LIKE '%music%')
TITLE                          | ARTIST                        
-------------------------------|-------------------------------
Handel: Music for the Royal... | English Concert & Trevor Pi...
Armada: Music from the Cour... | Fretwork                      
Purcell: Music for the Quee... | Equale Brass Ensemble, John...
Mozart: Chamber Music          | Nash Ensemble             

albums_to_delete.destroy_all
  TRANSACTION (0.2ms)  begin transaction
  Album Destroy (11.6ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 662]]
  TRANSACTION (6.2ms)  commit transaction
  TRANSACTION (0.1ms)  begin transaction
  Album Destroy (0.4ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 666]]
  TRANSACTION (2.0ms)  commit transaction
  TRANSACTION (0.1ms)  begin transaction
  Album Destroy (1.1ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 680]]
  TRANSACTION (2.4ms)  commit transaction
  TRANSACTION (0.1ms)  begin transaction
  Album Destroy (0.8ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 693]]
  TRANSACTION (2.2ms)  commit transaction
=> 
[#<Album:0x000000011b8c9b20
  id: 662,
  title: "Handel: Music for the Royal Fireworks (Original Version 1749)",
  artist: "English Concert & Trevor Pinnock",
  created_at: Wed, 25 Oct 2023 13:41:06.272419000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 13:41:06.272419000 UTC +00:00>,
 #<Album:0x000000011b8c99e0
  id: 666,
  title: "Armada: Music from the Courts of England and Spain",
  artist: "Fretwork",
  created_at: Wed, 25 Oct 2023 13:41:06.495982000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 13:41:06.495982000 UTC +00:00>,
 #<Album:0x000000011b8c98a0
  id: 680,
  title: "Purcell: Music for the Queen Mary",
  artist: "Equale Brass Ensemble, John Eliot Gardiner & Munich Monteverdi Orchestra and Choir",
  created_at: Wed, 25 Oct 2023 13:41:07.272272000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 13:41:07.272272000 UTC +00:00>,
 #<Album:0x000000011b8c9760
  id: 693,
  title: "Mozart: Chamber Music",
  artist: "Nash Ensemble",
  created_at: Wed, 25 Oct 2023 13:41:07.992990000 UTC +00:00,
  updated_at: Wed, 25 Oct 2023 13:41:07.992990000 UTC +00:00>]
irb(main):025> 

Combien y a t'il d'albums écrits par AC/DC ?
acdc_count = Album.where(artist: "AC/DC").count
Album Count (1.0ms)  SELECT COUNT(*) FROM "albums" WHERE "albums"."artist" = ?  [["artist", "AC/DC"]]
=> 2

Combien de chanson durent exactement 158589 millisecondes ?
count = Track.where(duration: 158589).count
  Track Count (1.8ms)  SELECT COUNT(*) FROM "tracks" WHERE "tracks"."duration" = ?  [["duration", 158589]]
=> 0

NIVEAU DIFFICILE

puts en console tous les titres de AC/DC.
acdc_tracks = Track.where(artist: "AC/DC")
  Track Load (1.5ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."artist" = ? /* loading for pp */ LIMIT ?  [["artist", "AC/DC"], ["LIMIT", 11]]
=> 
[#<Track:0x000000011b50d220

irb(main):032* acdc_tracks.each do |track|
irb(main):033*   puts track.title
irb(main):034> end
  Track Load (0.4ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."artist" = ?  [["artist", "AC/DC"]]

For Those About To Rock (We Salute You)
Put The Finger On You
Lets Get It Up
Inject The Venom
Snowballed
Evil Walks
C.O.D.
Breaking The Rules
Night Of The Long Knives
Spellbound
Go Down
Dog Eat Dog
Let There Be Rock
Bad Boy Boogie
Problem Child
Overdose
Hell Aint A Bad Place To Be
Whole Lotta Rosie


puts en console tous les titres de l'album "Let There Be Rock".
ltbr_tracks = Track.where(album: "Let There Be Rock")
  Track Load (0.4ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ? /* loading for pp */ LIMIT ?  [["album", "Let There Be Rock"], ["LIMIT", 11]]
=> 
[#<Track:0x000000011ba2e740
...
irb(main):037* ltbr_tracks.each do |track|
irb(main):038*   puts track.title
irb(main):039> end
  Track Load (0.2ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ?  [["album", "Let There Be Rock"]]
Go Down
Dog Eat Dog
Let There Be Rock
Bad Boy Boogie
Problem Child
Overdose
Hell Aint A Bad Place To Be
Whole Lotta Rosie

Calcule le prix total de cet album ainsi que sa durée totale.
rb(main):040> tracks = Track.where(album: "Let There Be Rock")
  Track Load (0.6ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ? /* loading for pp */ LIMIT ?  [["album", "Let There Be Rock"], ["LIMIT", 11]]
=> 
[#<Track:0x000000011bbac450
...
irb(main):041> total_price = 0.0
=> 0.0
irb(main):042* tracks.each do |track|
irb(main):043*   total_price += track.price
irb(main):044> end
  Track Load (0.2ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ?  [["album", "Let There Be Rock"]]

 total_price
=> 7.920000000000001 

total_duration
=> 2453259.0

Calcule le coût de l'intégralité de la discographie de "Deep Purple".
deep_purple_tracks = Track.where(artist: "Deep Purple")
  Track Load (0.2ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."artist" = ? /* loading for pp */ LIMIT ?  [["artist", "Deep Purple"], ["LIMIT", 11]]
=> 
[#<Track:0x000000011baa7460
...
irb(main):062> deep_purple_total_cost =0.0
=> 0.0
irb(main):063* deep_purple_tracks.each do |track|
irb(main):064*   deep_purple_total_cost += track.price
irb(main):065> end

deep_purple_total_cost
=> 90.0899999999999
 
Modifie (via une boucle) tous les titres de "Eric Clapton" afin qu'ils soient affichés avec "Britney Spears" en artist.
eric_tracks = Track.where(artist: "Eric Clapton")
irb(main):072* eric_tracks.each do |track|
irb(main):073*   track.update(artist: "Britney Spears")
irb(main):074> end

