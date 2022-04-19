#### Easter Egg!
Now that you've successfully made it this far, here's a little not-so-hidden easter egg for your enjoyment!
`echo "R3JlYXQgam9iISEgWW91J3JlIG5vdyBhIERvY2tlciBwcm8h" > egg && docker run --rm grycap/cowsay /usr/games/cowsay "$(base64 -d < egg)"`{{execute}}