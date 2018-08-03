/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';

import { Image } from 'react-native';
import {
  Container,
  Card,
  CardItem,
  Content,
  Header,
  Item,
  Input,
  Text,
  Button,
  Icon,
  Fab,
  Thumbnail,
  Body,
  Left,
  Right
} from 'native-base';
import axios from 'axios';

class App extends Component {

  constructor() {
    super();
    this.state = { active: true, search: "", dataResto: [], };
  }

  getApi = () => {
    var url = `https://developers.zomato.com/api/v2.1/search?q=${this.state.search}`;

    var config = {
      headers: { 'user-key': '9c1d934adf6d194ea0a5aff36b670d8c' }
    };

    axios.get(url, config)
      .then((getData) => {
        this.setState({ dataResto: getData.data.restaurants })
        console.log(this.state.dataResto[0].restaurant.name);
      })
  }
  render() {
    const data = this.state.dataResto.map((a, index) => {
      var restoNama = a.restaurant.name;
      var restoKota = a.restaurant.location.city;
      var restoAlamat = a.restaurant.location.address;
      // price / 2 untuk mendapatkan avarage cost 1 porsi
      var restoCurrency = a.restaurant.currency;
      var restoHarga = (a.restaurant.average_cost_for_two / 2);

      if (a.restaurant.thumb !== ''){
        var restoImage = a.restaurant.thumb;
      }

      // akan mengisi mengganti thumbnail restoran apabila thumbnail restoran kosong
      // no images available
      else if  (a.restaurant.thumb == ''){
      var restoImage = 'http://ipindiaservices.gov.in/GirPublic/writereaddata/images/No_Image_Available.jpg';
      }


      return (
        // Card untuk daftar makanan
        <Card key={index} style={{ flex: 0 }}>

        {/* Card item 1 (Thumbnail Resto) */}
          <CardItem style={{ backgroundColor: '#F4ECF9'}}> 
            <Left>
              {/*Thumbnail nama restoran */}
              <Thumbnail source={{ uri: restoImage }} />

              <Body>
                {/* Nama Restoran dan kota */}
                <Text>{restoNama}</Text>
                <Text note>{restoKota}</Text>
              </Body>
            </Left>

            <Right>
              {/* Harga makanan, pada database sudah dibagi 2 */}
              <Text>{restoCurrency} {restoHarga}</Text>
            </Right>

          </CardItem>

          <CardItem>
            <Body>
              <Image source={{ uri: restoImage }} style={{ height: 200, width: 325, flex: 1}} />
            </Body>
          </CardItem>

          <CardItem style={{ backgroundColor: '#F4ECF9'}}>
            <Left>
              <Icon name="md-locate" />
              <Text>{restoAlamat}</Text>
            </Left>
          </CardItem>

        </Card>
      )
    })



    return (

      <Container>
        {/* header search */}
        <Header searchBar rounded style={{ backgroundColor: '#AF7191' }}>
          <Item>
            <Icon name="search" />
            <Input placeholder="Cari menu makanan" onChangeText={(x)=> {this.setState({search: x})}} value={this.state.form}/>
          </Item>
        </Header>

        <Header style={{ backgroundColor: '#AF7191' }}>
          <Button block onPress={() => { this.getApi() }} style={{ backgroundColor: '#803F9F' }}>
            <Text> LIHAT DAFTAR RESTO </Text>
          </Button>
        </Header>

        <Content style={{ backgroundColor: '#EACEDC' }}>
          {data}
        </Content>

        {/* Fab Share */}
        <Fab
          active={this.state.active}
          direction="up"
          style={{ backgroundColor: '#803F9F' }}
          position="bottomRight"
          onPress={() => this.setState({
            active: !this.state.active
          })}
        >
          <Icon name="share" />
        </Fab>
      </Container>

    )
  }
}
export default App;
