## Searh API

### address in Korea

#### 지번
  - old style but goverment system use it
  - pnu is unique number of address which based on 지번

##### pnu
  - 19 digit number representing 지번

![](/assets/images/pnu_detail.png)

#### 도로명
  - new style like western address system

#### ex
  - 지번 : 화성시 반송동 80
  - 도로명 : 화성시 동탄중앙로 189


### Scenario

1. search address
![](/assets/images/weprops_input.png)

```
curl -v -X GET "https://dapi.kakao.com/v2/local/search/address.xml" \
--data-urlencode "query=반송동 80" \
-H "Authorization: KakaoAK ef1da2c12cb3824dac99435cd1c4a666" 
```

  > [result](#address-search)


2. if no result then search keyword

```
curl -v -X GET "https://dapi.kakao.com/v2/local/search/address.xml" \
--data-urlencode "query=동탄 월드메르디앙" \
-H "Authorization: KakaoAK ef1da2c12cb3824dac99435cd1c4a666"
```

  > [result](#keyword-search)

3. show list

![](/assets/images/weprops_list.png)

4. user select then search address using address_name

5. save data


### data structure

  - addr : String (mandatory)
  - addr_sub : String (optional)
  - building_name : String (optioanl)
  - pnu : String (mandatory)
  - shape : String (mandatory) : apt, flat, land, house
  - own_type : String (mandatory) : own, renter, pinned
  - pinned_area : Array (optional) : [59, 84, 112]
  - loan_info : todo

#### data in xml
  > [reference](#address-search)

  - addr : /result/documents/address_name
  - addr_sub : user input
  - building_name : /result/documents/road_address/building_name
  - pnu : 
    - /result/documents/address/b_code : 10 digit
    - /result/documents/address/mauntain_yn : N => 1 / Y => 0
    - /result/documents/address/main_address_no : 4 digit (ex. 80 => 0080)
    - /result/documents/address/sub_address_no : 4 digit (ex. 1 => 0001)
  - shape : user input
  - pinned_area : user input

  #### ex)

  ##### data.1

  - addr : 경기 화성시 반송동 80
  - addr_sub : 346-1103
  - building_name : 동탄시범다은마을 월드메르디앙반도유보라
  - pnu : 4159012700100800000
  - shape : apt
  - own_type : own

##### data.2

  - addr : 서울시 반포동 2-12
  - addr_sub : 
  - build_name : 아크로리버파크
  - pnu : 1165010700100020012
  - share : apt
  - own_type : pinned
  - pinned_area : [59, 84, 112]




### xml result



#### address search

```
curl -v -X GET "https://dapi.kakao.com/v2/local/search/address.xml" \
--data-urlencode "query=반송동 80" \
-H "Authorization: KakaoAK ef1da2c12cb3824dac99435cd1c4a666" 
```

```
<result>
  <meta>
    <is_end>true</is_end>
    <total_count>1</total_count>
    <pageable_count>1</pageable_count>
  </meta>
  <documents>
    <road_address>
      <undergroun_yn>N</undergroun_yn>
      <road_name>동탄중앙로</road_name>
      <underground_yn>N</underground_yn>
      <region_2depth_name>화성시</region_2depth_name>
      <zone_no>18438</zone_no>
      <sub_building_no></sub_building_no>
      <region_3depth_name>반송동</region_3depth_name>
      <main_building_no>187</main_building_no>
      <address_name>경기 화성시 동탄중앙로 187</address_name>
      <y>37.20203862370245</y>
      <x>127.06682015318336</x>
      <region_1depth_name>경기</region_1depth_name>
      <building_name>동탄시범다은마을 월드메르디앙반도유보라</building_name>
    </road_address>
    <address_name>경기 화성시 반송동 80</address_name>
    <address>
      <b_code>4159012700</b_code>
      <region_3depth_h_name>동탄1동</region_3depth_h_name>
      <main_address_no>80</main_address_no>
      <h_code>4159058500</h_code>
      <region_2depth_name>화성시</region_2depth_name>
      <main_adderss_no>80</main_adderss_no>
      <sub_address_no></sub_address_no>
      <region_3depth_name>반송동</region_3depth_name>
      <address_name>경기 화성시 반송동 80</address_name>
      <y>37.2024717181881</y>
      <x>127.06576618545195</x>
      <mountain_yn>N</mountain_yn>
      <zip_code>445160</zip_code>
      <region_1depth_name>경기</region_1depth_name>
      <sub_adderss_no></sub_adderss_no>
    </address>
    <y>37.2024717181881</y>
    <x>127.06576618545195</x>
    <address_type>REGION_ADDR</address_type>
  </documents>
</result>
```

#### keyword search

```
curl -v -X GET "https://dapi.kakao.com/v2/local/search/address.xml" \
--data-urlencode "query=동탄 월드메르디앙" \
-H "Authorization: KakaoAK ef1da2c12cb3824dac99435cd1c4a666"
```

```
<result>
  <documents>
    <address_name>경기 화성시 반송동 212</address_name>
    <category_group_code></category_group_code>
    <category_group_name></category_group_name>
    <category_name>부동산 &gt; 주거시설 &gt; 아파트</category_name>
    <distance></distance>
    <id>11506035</id>
    <phone></phone>
    <place_name>나루마을동탄월드메르디앙반도유보라2차아파트</place_name>
    <place_url>http://place.map.daum.net/11506035</place_url>
    <road_address_name>경기 화성시 동탄나루로 55</road_address_name>
    <x>127.07834397869306</x>
    <y>37.19151808618851</y>
  </documents>
  <documents>
    <address_name>경기 화성시 반송동 80</address_name>
    <category_group_code></category_group_code>
    <category_group_name></category_group_name>
    <category_name>부동산 &gt; 주거시설 &gt; 아파트</category_name>
    <distance></distance>
    <id>11506013</id>
    <phone></phone>
    <place_name>시범다은마을동탄월드메르디앙반도유보라아파트</place_name>
    <place_url>http://place.map.daum.net/11506013</place_url>
    <road_address_name>경기 화성시 동탄중앙로 187</road_address_name>
    <x>127.065667189762</x>
    <y>37.2026231497371</y>
  </documents>
  ...
</result>
```