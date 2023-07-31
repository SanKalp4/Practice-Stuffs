# Step 1: Define a function to process each HTML tag according to its meaning
def process_a(tag):
    # Process <a> tags as links
    return f"[{tag.get_text()}]({tag['href']})"

def process_br(tag):
    # Process <br> tags as line breaks
    return '\n'

def process_div(tag):
    # Process <div> tags as block containers
    return f"\n\n{tag.get_text()}\n\n"

def process_em(tag):
    # Process <em> tags as emphasized text (italics)
    return f"*{tag.get_text()}*"

def process_li(tag):
    # Process <li> tags as list items
    return f"- {tag.get_text()}"

def process_ol(tag):
    # Process <ol> tags as ordered lists
    items = [f"{i}. {item.get_text()}" for i, item in enumerate(tag.find_all('li'), 1)]
    return '\n'.join(items)

def process_p(tag):
    # Process <p> tags as paragraphs
    return f"\n{tag.get_text()}\n"

def process_s(tag):
    # Process <s> tags as strikethrough text
    return f"~~{tag.get_text()}~~"

def process_span(tag):
    # Process <span> tags (no specific meaning, just get the text)
    return tag.get_text()

def process_strong(tag):
    # Process <strong> tags as strong/bold text
    return f"**{tag.get_text()}**"

def process_u(tag):
    # Process <u> tags as underlined text
    return f"<u>{tag.get_text()}</u>"

def process_ul(tag):
    # Process <ul> tags as unordered lists
    items = [f"- {item.get_text()}" for item in tag.find_all('li')]
    return '\n'.join(items)

# Step 2: Define a mapping of tag names to their respective processing functions
tag_processing_map = {
    'a': process_a,
    'br': process_br,
    'div': process_div,
    'em': process_em,
    'li': process_li,
    'ol': process_ol,
    'p': process_p,
    's': process_s,
    'span': process_span,
    'strong': process_strong,
    'u': process_u,
    'ul': process_ul,
}

# Step 3: Apply the function to 'html_summary' column based on the tag name
def process_html_tags(text):
    soup = BeautifulSoup(text, 'html.parser')

    for tag in soup.find_all():
        tag_name = tag.name
        if tag_name in tag_processing_map:
            processed_text = tag_processing_map[tag_name](tag)
            tag.string = processed_text

    return soup.get_text()

# Step 4: Apply the function to 'html_summary' column and store the result in a new column
df['processed_summary']= df['html_summary'].apply(process_html_tags)
