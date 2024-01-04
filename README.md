# Export html with self contained images from Typora
 Script for exporting an html with embedded images from Typora.
 Copy and modified from other professionist: please see [github.com/typora/typora-issues/issues/3656](https://github.com/typora/typora-issues/issues/3656)

``` python
def encode_images_to_base64(input_file, output_file):
    """This function embed images into html export of Typora"""
    with open(input_file, 'r', encoding='utf-8') as md_file:
        html_content = md_file.read()

    # Find all image tags in the markdown content
    img_pattern = r'<img[^>]*\ssrc="([^"]+)"[^>]*>'
    image_tags = re.findall(img_pattern, html_content)

    for image_path in image_tags:
        image_path_temp=image_path
        image_path=image_path.replace("./","\\")
        image_path=image_path.replace("/","\\")
        prefix=r"C:\Users\pietr\Documents\STADATA\GitHub\cv"
        image_path=prefix+image_path
        log_progress("Ciclo For image_path")
        log_progress(image_path)
        # Check if the image path is relative or absolute
        if not os.path.isabs(image_path):
            image_path = os.path.join(os.path.dirname(input_file), image_path)
            log_progress("IF primo")
            log_progress(image_path)
        if os.path.exists(image_path):
            with open(image_path, 'rb') as image_file:
                # Encode the image content to base64
                image_base64 = base64.b64encode(image_file.read()).decode('utf-8')
                log_progress("IF secondo, dentro With")
                log_progress(image_path)
                # Replace the image path with the base64 encoded content in the markdown content
                html_content = html_content.replace(image_path_temp, f'data:image/png;base64,{image_base64}')
    # Write the updated markdown content to the output file
    with open(output_file, 'w', encoding='utf-8') as html_file:
        html_file.write(html_content)
```