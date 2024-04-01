import requests
from bs4 import BeautifulSoup

#url = "https://mkp.gem.gov.in/furniture-and-furnishings-accommodation-furniture-furniture-revolving-chair-v3-/search?q[]=chair&sort_type=price_in_asc&page=5&_xhr=1"

headers = {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate, br",
    "Accept-Language": "en-GB,en-US;q=0.9,en;q=0.8",
    "Connection": "keep-alive",
    "Cookie": "GEMDCPROD=NODE2; XSRF-TOKEN=Ah714coJwn5l6RwGhESzsYmz4FS%2BCoy7RCuL%2FEy0lu4%3D; idleTimerLoggedOut=true; __msi__=0717e0c897c3ffc82e2251a81ef0b093; _site_session=7e13f51cf96a2c298c4860423711cc35; TS01024d38=015c77a21c276bc0cf55eee3ed4eb10cf0f668ea73a11af06e3e257d0727ae855dc25722c0f1524986af732620e026c4c0c1aa58f55003ace5204f6118c8c057176e601b5f; _ga=GA1.3.2058497873.1702985379; _gid=GA1.3.877016822.1702985379; GEMDCPROD=NODE2; prod_hist=%5B%225116877-68605025204%22%5D; themeOption=0; TS01921550=015c77a21c85ba80d1bbe9bcb9377551e71e7fe8bee0cb8ee3c5f31ad2e12cc3594f37cd21e40d4016cebfd25a0a41ad6910b41be00a1c0bea6f5e94e274c010b39a074e4dff96e794fac1443b692406106626b3bd0409326629303ca35f0afefd1d8a3d73bee3b07c6c9cc48ff1a202a9dc08da5bd69f5c58584c1afa81e59548ca8cc325; __bp__=medium; pref=%7B%22search_view_type%22%3A%22grid-view%22%7D",
    "Host": "mkp.gem.gov.in",
    "Referer": "https://mkp.gem.gov.in/furniture-and-furnishings-accommodation-furniture-furniture-revolving-chair-v3-/search",
    "Sec-Ch-Ua": "\"Not_A Brand\";v=\"8\", \"Chromium\";v=\"120\", \"Google Chrome\";v=\"120\"",
    "Sec-Ch-Ua-Mobile": "?0",
    "Sec-Ch-Ua-Platform": "\"Windows\"",
    "Sec-Fetch-Dest": "empty",
    "Sec-Fetch-Mode": "cors",
    "Sec-Fetch-Site": "same-origin",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
    "X-Csrf-Token": "Ah714coJwn5l6RwGhESzsYmz4FS+Coy7RCuL/Ey0lu4=",
    "X-Requested-With": "XMLHttpRequest",
}
def scrape_gem_page(url):
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        product_divs = soup.find_all('div', class_='variant-wrapper')
        for product_div in product_divs:
                    #seller_info = product_div.find('div', class_='seller-info')
                    seller_type = product_div.find('span', class_='sold_as_summary').text.strip()
                    if seller_type == 'Resellers':
                        product_title = product_div.find('span', class_='variant-title').text.strip()
                        seller= 'Resellers'
                        brand_element = product_div.find('div', class_='variant-brand')
                        brand = brand_element.text.strip().replace('Brand: ', '') if brand_element else ''
                        #brand = product_div.find('div', class_='variant-brand').text.strip().replace('Brand: ', '') if brand_element else ''
                        min_qty = product_div.find('div', class_='variant-moq').text.strip()
                        min_qty_final = int(''.join(filter(str.isdigit, min_qty.strip())))
                        price = product_div.find('span', class_='variant-final-price').text.strip()
                        link = product_div.find('div',class_='variant-image').a['href']
                        img_tag = product_div.find('img')
                        image_link = img_tag.get('src')
                        #print(f"Product: {product_title}\nBrand: {brand}\Seller: {seller_type}\nMin. Qty: {min_qty_final}\nPrice: {price}\nImg: {image_link}\nLink to Product: https://mkp.gem.gov.in/{link}\n{'='*30}")
                        with open('output.txt', 'a', encoding='utf-8') as file:
                            file.write(f"Product: {product_title}\nBrand: {brand}\nSeller: {seller_type}\nMin. Qty: {min_qty_final}\nPrice: {price}\nImg: {image_link}\nLink to Product: https://mkp.gem.gov.in/{link}\n{'='*30}\n")

                    else:
                            pass
        
        return True
    else:
        print(f"Failed to retrieve the page. Status code: {response.status_code}")
        return False


base_url="https://mkp.gem.gov.in/furniture-and-furnishings-accommodation-furniture-furniture-revolving-chair-v3-/search?q[]=chair&sort_type=price_in_asc"
for page_number in range(1, 3577):
    page_url = f"{base_url}&page={page_number}&_xhr=1"
    print(f"Scraping page {page_number}...")
    if not scrape_gem_page(page_url):
        break  # Break if there's an issue with a page


# Read product data from the text file
with open('output.txt', 'r') as file:
    product_data = file.read().split("==============================\n")

# Generate HTML content dynamically
html_content = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Product Information</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }

        .product-card {
            display: flex;
            background-color: #fff;
            border: 1px solid #ddd;
            border-radius: 8px;
            margin-bottom: 20px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .product-details {
            flex: 1;
            padding: 15px;
        }

        .product-details h2 {
            color: #333;
        }

        .product-details p {
            margin: 0;
            color: #666;
        }

        .product-link {
            margin-top: 10px;
        }

        .product-image {
            max-width: 200px;
            height: auto;
            border-top-right-radius: 8px;
            border-bottom-right-radius: 8px;
        }
    </style>
</head>
<body>
"""

for product_info in product_data:
    if product_info.strip():
        product_lines = product_info.strip().split('\n')
        product_name = product_lines[0].split(': ')[1]
        brand = product_lines[1].split(': ')[1]
        seller = product_lines[2].split(': ')[1]
        Min_Quantity= product_lines[3].split(': ')[1]
        price = product_lines[4].split(': ')[1]
        Img_link = product_lines[5].split(': ')[1]
        link = product_lines[6].split(': ')[1]

        html_content += """
        <div class="product-card">
            <div class="product-details">
                <h2>{}</h2>
                <p><strong>Brand:</strong> {}</p>
                <p><strong>Seller:</strong> {}</p>
                <p><strong>Min. Qty:</strong> {}</p>
                <p><strong>Price:</strong> {}</p>
                <div class="product-link"><strong>Link to Product:</strong> <a href="{}" target="_blank">View Product</a></div>
            </div>
            <img src="{}" alt="Product Preview" class="product-image">
        </div>
        """.format(product_name, brand,seller, Min_Quantity, price,link, Img_link)

html_content += """
</body>
</html>
"""

# Write the generated HTML to a file
with open('product_page.html', 'w') as file:
    file.write(html_content)

print("HTML file generated successfully.")