import scrapy
from scrapy_splash import SplashRequest

class DONALDS(scrapy.Spider):
    name = 'donalds'
    start_urls = ['https://shop.donaldson.com/store/en-us/search?N=2719494061&catNav=true']

    def start_requests(self):
        for url in self.start_urls:
            yield SplashRequest(url, self.parse, args={'wait': 2})

    def parse(self, response):

        products = response.css('div.listTile.col-lg-12.col-md-12.col-sm-12.col-xs-12.no-padding')
        for product in products:
            product_name = product.css('a.commonLinkClass.donaldson-part-details::text').get()
            short_description = product.css('span.breakWordTable.donaldson-product-details::text').get()
            availability = product.css('span.priceAndInventoryCallCheck.no-padding.NotToGoToPDP p.contactUsMsg::text').get()

            yield {
                    "product": product_name.strip() if product_name else None,
                    "short Description": short_description.strip() if short_description else None,
                    "Availability": availability.strip() if availability else None,
                    }

