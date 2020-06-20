## We can create multiple docker file's in same working directory during build building of the image specified from docker file.

We have to pass the argument -f  <docker-file-name >  build command.

docker build -t (tag-name) -f (docker-file-name-1)  .(current directory)

docker build -t <tag-name> -f <docker-file-name-2>  .(current directory)


If we dont specify specific docker file name , it will take up default docker-file  which is in present working directory
