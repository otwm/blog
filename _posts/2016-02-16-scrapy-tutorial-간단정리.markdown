---
layout: post
title:  "scrapy 간단정리"
comments: True
---
## 스크래피의 간단설명  

## 스크래피 익셉션  
Build-in Exceptions reference  
스크래피의 예외 리스트와 사용법  

* DropItem    
파이프라인 에서 프로세스를 멈추게 하는 익셉션
* CloseSpider
이 익셉션은 스파이더 콜백으로 부터 스파이더가 멈추거나 닫혀지는 요청을 할 때 발생한다.  
예를들면   


    def parse_page(self, response):
      if 'Bandwidth exceeded' in response.body:
          raise CloseSpider('bandwidth_exceeded')          
* IgnoreRequest  
  
* NotConfigured
* NotSupported



## 스크래피 튜토리얼   
스크래피 프로젝트 생성    

      scrapy startproject tutorial

아이템 작성(도메인 객체 정도라고 생각 하면 되겠다.)    

      import scrapy

      class DmozItem(scrapy.Item):
          title = scrapy.Field()
          link = scrapy.Field()
          desc = scrapy.Field()

스파이더 작성(파스 메서드가 실제 크롤링 한다.)  

          import scrapy

          class DmozSpider(scrapy.Spider):
              name = "dmoz"
              allowed_domains = ["dmoz.org"]
              start_urls = [
                  "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/",
                  "http://www.dmoz.org/Computers/Programming/Languages/Python/Resources/"
              ]

              def parse(self, response):
                  filename = response.url.split("/")[-2] + '.html'
                  with open(filename, 'wb') as f:
                      f.write(response.body)

크롤링 커맨드  

    scrapy crawl dmoz

스크래피 쉘을 이용한 response test(실제로 프로그래밍전에 어떤식으로 파싱할지를 계획하기에 좋다.)  

    scrapy shell "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/"

xpath와 css 셀렉터 문법으로 응답 정보를 추출한다.  

    response.xpath('//ul/li')

    for sel in response.xpath('//ul/li'):
      title = sel.xpath('a/text()').extract()
      link = sel.xpath('a/@href').extract()
      desc = sel.xpath('text()').extract()
      print title, link, desc

아이템 저장  

      scrapy crawl dmoz -o items.json          
## 아이템 파이프라인  

한마디로 말하면 AOP와 비슷하다고 할 수 있겠다. 스파이더에 의해 스크랩 된 이후에 다수의 컴퍼넌트가 연속적으로 실행되며 크롤링 이후의 관심사 처리라고 생각하면 된다. 심지어 elasticsearch에 대한 파이프라인도 깃헙에 존재한다. 공식문서에서는 몽고디비와 연동하는 것을 보여주고 있다. 파이프라인 클래스에서 정의 해야하는 메서드는 다음과 같다.  

* process_item(self, item, spider)  
이 메서드는 모든 아이템 파이프라인 호출 시 실행된다. 반환은 Item 클래스나 DropItem을 라이스하거나 하여야 된다. DropItem은 에러 처리라고 보면 된다. 이것이 라이즈 되면 해당 파이프라인의 실행을 마치며 더 이상의 파이프 라인 컴퍼넌트가 실행되지 않는다.  

* open_spider(self, spider)
이 메서드는 스파이더가 오픈 될 때 실행된다.  

* close_spider(self, spider)  
이 메서드는 스파이더가 닫힐 때 실행된다.

* from_crawler(cls, crawler)  

크롤러로 부터 파이프라인이 생성 될 때 실행된다. 물론 파이프 라인이 존재하지 않는 다면 미실행. 반드시 새로운 파이프라인 인스텉스를 반환해야 한다. 크롤러 객체는 모든 스크래피의 코어 컴퍼넌트로 접근이 가능하다.

    from scrapy.exceptions import DropItem

    class PricePipeline(object):

        vat_factor = 1.15

        def process_item(self, item, spider):
            if item['price']:
                if item['price_excludes_vat']:
                    item['price'] = item['price'] * self.vat_factor
                return item
            else:
                raise DropItem("Missing price in %s" % item)
활성화
0부터 1000까지의 숫자를 주어 우선 순위를 결정한다.  

        IT화EM_PIPELINES = {
            'myproject.pipelines.PricePipeline': 300,
            'myproject.pipelines.JsonWriterPipeline': 800,
        }  
