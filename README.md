# Reproductor_Musical_Java
Proyecto de reproductor musical en java 
Este proyecto se divide en 4 clases las cuales son: 
----------
-Canciones.
----------
En esta clase encontraremos las caracteristicas que tiene una cancion como su nombre y el artista tambien guardara la informacion sobre si es o no una cancion favorita.
por lo tanto tendremos los gety set de cada cosa junto con el metodo para mostrar la informacion de cada cancion.
## Codigo.
``` java
package com.mycompany.reproductor;

public class Cancion {
    private String titulo;
    private String artista;
    private boolean Favorita;

    public Cancion(String titulo, String artista) {
        this.titulo = titulo;
        this.artista = artista;
        this.Favorita = false;
    }

    public String getTitulo() {
        return titulo;
    }

    public String getArtista() {
        return artista;
    }

    public boolean isFavorita() {
        return Favorita;
    }

    public void setTitulo(String titulo) {
        this.titulo = titulo;
    }

    public void setArtista(String artista) {
        this.artista = artista;
    }

    public void setFavorita(boolean Favorita) {
        this.Favorita = Favorita;
    }
    
    public String mostrarInfo(){
    return "Cancion: " + titulo + "--" + artista + "Cancion Favorita: " + Favorita;
    }
    
    
}
```
----------
-Reproductor.
----------
En esta clase tenemos todos los metodos y funcionalidades que nos piden como se muestra en el siguiente codigo.
```java
package com.mycompany.reproductor;

public class Reproductor {
    
    private ListaCanciones listaCanciones;
    private boolean pausado;
    private int cancionReproduciendo;

    public Reproductor(ListaCanciones listaCanciones) {
        this.listaCanciones = listaCanciones;
        this.pausado = true;
        this.cancionReproduciendo = 0;
    }
    
    public void proximaCancion() {
        cancionReproduciendo = (cancionReproduciendo + 1) 
        % listaCanciones.getTotalCanciones();
        System.out.println("Siguiente cancion: " + 
        getCancionActual().mostrarInfo());
    }
    
    public void devolverCancion(){
        cancionReproduciendo = (cancionReproduciendo + listaCanciones.getTotalCanciones()-1) 
                % listaCanciones.getTotalCanciones();
        System.out.println(" Cancion Anterior: " + getCancionActual().mostrarInfo());
    }
    
       public void cambiarEstadoReproduccion() {
        pausado = !pausado;
        if (pausado) {
            System.out.println("Pausado: " + getCancionActual().getTitulo());
        } else {
            System.out.println("Reproduciendo: " + getCancionActual().getTitulo());
        }
    }
    
    public void agregarCancionFavorita() {
        listaCanciones.marcarFavorita(cancionReproduciendo);
    }
    
        public Cancion getCancionActual() {
        return listaCanciones.getCancion(cancionReproduciendo);
    }
    
    public void mostrarEstado() {
    System.out.println("\n=============================");
    System.out.println("ESTADO DEL REPRODUCTOR");
    System.out.println("=============================");
    System.out.println("Cancion actual : " + getCancionActual().mostrarInfo());
    System.out.println("Estado         : " + (pausado ? "Pausado" : "Reproduciendo"));
    System.out.println("=============================\n");
    }
    
    public boolean isPausado() {
        return pausado;
    }

    public int getCancionReproduciendo() {
        return cancionReproduciendo;
    }
}
```
----------
-ListaCanciones.
----------
En nuestra clase lista encontraremos la creacion del Array necesario para guarda nuestras canciones y los metodos para mostrar nuestras listas como las de canciones y favoritas, y funciones como agregar canciones y marcar como favorita.
```java
package com.mycompany.reproductor;

public class ListaCanciones {
    private Cancion[] canciones;
    private int totalCanciones;

    public ListaCanciones(int capacidadMaxima) {
        this.canciones = new Cancion[capacidadMaxima];
        this.totalCanciones = 0;
    }
    
    public void agregarCancion(Cancion cancion){
    if (totalCanciones < canciones.length){
        canciones[totalCanciones]= cancion;
        totalCanciones++;
        System.out.println("Cancion: " + cancion.getTitulo() + "agregada con exito");
    }
    else{
        System.out.println("No se puede agregar la cancion porque la lista esta llena.");
    }
    }
    
    
    public Cancion getCancion(int indice){
        if(indice >= 0 && indice < totalCanciones){
            return canciones[indice];
        }
        return null;
    }
    
    public void marcarFavorita(int indice){
        Cancion cancion = getCancion(indice);
        if (cancion != null){
            if(!cancion.isFavorita()){
                cancion.setFavorita(true);
                System.out.println("Cancion: "+cancion.getTitulo()+" Agreagada a Favoritos.");
            }
            else{
                System.out.println("Cancion: " + cancion.getTitulo()+" Ya esta en favoritos.");
            }
        }
    }
    
    public void mostrarFavoritas(){
        System.out.println("\n== CANCIONES FAVORITAS ===");
        boolean hayFavoritas = false;
        for (int i = 0; i < totalCanciones; i++) {
            if(canciones[i].isFavorita()){
                System.out.println(" " + canciones[i].mostrarInfo());
                hayFavoritas = true;
                }
            }
        if (!hayFavoritas){
            System.out.println(" No hay canciones favoritas aún. ");
            }
        }
    
    public void mostrarTodas(){
        System.out.println("\n=== TODAS LAS CANCIONES ===");
        for(int i=0; i<totalCanciones; i++){
            System.out.println(" [" + i + "] " + canciones[i].mostrarInfo());
        }
    }
    
    public int getTotalCanciones() {
        return totalCanciones;
    }
}
```
----------
-Main.
---------
Por ultimo en nuestro Main tendremos nuestros llamados a los diferentes metodos y la creacion de cada cancion.
```java
package com.mycompany.reproductor;

public class Main {
    public static void main(String[] args){
        
        Cancion c1 = new Cancion("Mil horas"," Los Abuelos de la Nada.");
        Cancion c2 = new Cancion("Por ese hombre"," Fandango.");
        Cancion c3 = new Cancion("A esa"," Franco De Vita.");
        Cancion c4 = new Cancion("Algo de mi"," Franco De Vita.");
        Cancion c5 = new Cancion("Si me dejas ahora"," Camilo Sesto.");
        
        ListaCanciones lista=new ListaCanciones(10);
        lista.agregarCancion(c1);
        lista.agregarCancion(c2);
        lista.agregarCancion(c3);
        lista.agregarCancion(c4);
        lista.agregarCancion(c5);
        lista.mostrarTodas();
        
        Reproductor reproductor = new Reproductor(lista);
        
        reproductor.mostrarEstado();
        reproductor.cambiarEstadoReproduccion();
        reproductor.proximaCancion();
        reproductor.devolverCancion();
        reproductor.agregarCancionFavorita();
        lista.mostrarFavoritas();
    }
}
```
---------
En cada clase se establecen los atributos y metodos necesarios para la ejecucion del codigo por medio de la terminal en un futuro se busca generar tambien una interfaz amigable y funcional.
