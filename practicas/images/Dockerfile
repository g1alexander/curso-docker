FROM ubuntu:latest

RUN touch /usr/src/test.txt

# esto no es recomendable, debido a que se esta generando una nueva capa opucando espacio en la imagen, es mejor ponerlo en la capa anterior
RUN rm /usr/src/test.txt