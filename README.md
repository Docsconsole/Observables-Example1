# Observables-Example1
Observables-Example1

"C:.
|   favicon.ico
|   index.html
|   main.ts
|   polyfills.ts
|   styles.css
|   test.ts
|
+---app
|   |   app.component.css
|   |   app.component.html
|   |   app.component.spec.ts
|   |   app.component.ts
|   |   app.module.ts
|   |
|   +---model
|   |       movie.model.ts
|   |
|   \---service
|           movie.service.ts
|
+---assets
|       .gitkeep
|
\---environments
        environment.prod.ts
        environment.ts"


export class Movie {
  id: Number;
  name: String;
  director: String;
}






import {Injectable} from '@angular/core';
import {Movie} from '../model/movie.model';

@Injectable({
  providedIn: 'root'
})
export class MovieService {

  movies: Movie[] = [{
    id: 10001,
    name: 'movie1',
    director: 'director1'
  },
    {
      id: 10002,
      name: 'movie2',
      director: 'director2'
    },
    {
      id: 10003,
      name: 'movie3',
      director: 'director3'
    },
    {
      id: 10004,
      name: 'movie4',
      director: 'director4'
    },
    {
      id: 10005,
      name: 'movie5',
      director: 'director5'
    }

  ];

  constructor() {
  }
  public getMovies():Movie[] {
    return this.movies;
  }
}


<h1>Observables Example</h1>
<p>For more details please look into console logs</p>


  import {Component, OnInit} from '@angular/core';
  import {Observable} from 'rxjs';
  import {MovieService} from './service/movie.service';
  
  @Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
  })
  export class AppComponent implements OnInit {
    title = 'Observables-Example1';
  
    constructor(private movieService: MovieService) {
    }
  
    ngOnInit(): void {
  
      const movies = this.movieService.getMovies();
      function seqSubscriber(observer) {
        let timeoutId;
  
        function doSequence(movies, idx) {
          timeoutId = setTimeout(() => {
            observer.next(movies[idx]);
            if (idx === movies.length - 1) {
              observer.complete();
            } else {
              doSequence(movies, ++idx);
            }
          }, 1000);
        }
        doSequence(movies, 0);
        return {
          unsubscribe() {
            clearTimeout(timeoutId);
          }
        };
      }
  
      const sequence = new Observable(seqSubscriber);
      sequence.subscribe({
        next(movie) {
          console.log(movie);
        },
        complete() {
          console.log('Finished sequence');
        }
      });
  
    }
  }




import {BrowserModule} from '@angular/platform-browser';
import {NgModule} from '@angular/core';

import {AppComponent} from './app.component';


@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {
}



Commnads to create the service,components and classes in angular.


ng g service service/Movie --skipTests=true
ng g component Acomp
ng g component Bcomp

ng g class model/movie --type=model --skipTests=true
ng g component seqsubscriber 
ng generate class model.movie --type=model






