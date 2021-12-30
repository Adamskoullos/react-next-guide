# Card

Basic structure of a Card component with all its child components. The below example includes the following:

1. `CardHeader` > Avatar, Title and Subtitle and Menu
2. `CardMedia` > Card Image (Could be a chart)
3. `CardContent` > Main card information
4. `CardActions`

```js
<Card sx={{ maxWidth: 345 }}>
  <CardHeader
    avatar={
      <Avatar sx={{ bgcolor: red[500] }} aria-label="recipe">
        R
      </Avatar>
    }
    action={
      <IconButton aria-label="settings">
        <MoreVertIcon />
      </IconButton>
    }
    title="Shrimp and Chorizo Paella"
    subheader="September 14, 2016"
  />
  <CardMedia
    component="img"
    height="194"
    image="/static/images/cards/paella.jpg"
    alt="Paella dish"
  />
  <CardContent>
    <Typography gutterBottom variant="h5" component="div">
      Lizard
    </Typography>
    <Typography variant="body2" color="text.secondary">
      Lizards are a widespread group of squamate reptiles, with over 6,000
      species, ranging across all continents except Antarctica
    </Typography>
  </CardContent>
  <CardActions>
    <Button size="small">Share</Button>
    <Button size="small">Learn More</Button>
  </CardActions>
</Card>
```
