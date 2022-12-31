## 0. Demo

sevima medsos

## 1. Installation

    git clone https://github.com/taufikhidayat1993/sevima_medsos.git
    cd sevima_medsos
    composer install
    php artisan key:generate
    npm install

### 1.2 Configuration

Check the .env.example file for db settings

### 1.3 Database

This will create the db structure

    php artisan migrate

## running

    cd sevima_medsos
    php artisan serve
    npm run dev
