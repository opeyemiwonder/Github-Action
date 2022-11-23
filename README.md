# Github-Action

on:
  push:
    branches: [ "dev" ]
jobs:
  
  run-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '19.1.0'
      - name: install packages
        run: yarn install
      - name: build application
        run: yarn build
        

  build:
    needs: run-tests
    runs-on: ubuntu-latest
    steps:
    - name: deploy to server
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        key: ${{ secrets.KEY }}
        port: ${{ secrets.PORT }}
        script: | 
          cd animatedtalkingheads.web
          ./deploy.sh
