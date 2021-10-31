%{
  version: "1.0.1",
  title: "பாங்கு பொருத்துதல்",
  excerpt: """
  எலிக்சரின் திறன்மிக்க அம்சங்களில் பாங்கு பொருத்துதலும் ஒன்றாகும். எளிய மதிப்புகளிலிருந்து, தரவுக்கட்டுருக்களையும், செயற்கூறுகளையும் கூட நம்மால் பொருத்திப்பார்க்கமுடியும். பாங்குகளைப்பொருத்துவதையும் அதன் பயன்களையும் இந்த பாடத்தில் கற்றுக்கொள்ளலாம்.
  """
}
---

## பொருத்தும் செயல்பாடு

ஓர் ஆச்சரியமான விசயம் தெரியுமா? எலிக்சரில், `=` என்பது உண்மையில் பொருத்தும் செயல்பாடு ஆகும். இயற்கணிதத்தின் சமனிலை குறியீட்டுக்கு ஒப்பானதாக இதைக்கருதலாம். இச்செயல்பாட்டைப்பயன்படுத்தும் கோவை, ஒரு சமன்பாடாக எடுத்துக்கொள்ளப்படுகிறது. அதன் இருபுறமும் உள்ள மதிப்புகளை எலிக்சர் பொருத்திப்பார்க்கிறது. அவையிரண்டும் பொருந்தும் பட்சத்தில், அதன் மதிப்பு திருப்பியனுப்பப்படுகிறது. இல்லையெனில், ஒரு வழுவைத்தருகிறது. ஒரு எடுத்துக்காட்டைப்பார்க்கலாம்:

```elixir
iex> x = 1
1
```

மேலுமோர் எளிய எடுத்துக்காட்டை முயன்றுபார்க்கலாம்:

```elixir
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

நாம் இதுவரையறிந்த தொகுப்புகளுக்கு பாங்குபொருத்திப்பார்க்கலாம்:

```elixir
# Lists
iex> list = [1, 2, 3]
[1, 2, 3]
iex> [1, 2, 3] = list
[1, 2, 3]
iex> [] = list
** (MatchError) no match of right hand side value: [1, 2, 3]

iex> [1 | tail] = list
[1, 2, 3]
iex> tail
[2, 3]
iex> [2 | _] = list
** (MatchError) no match of right hand side value: [1, 2, 3]

# Tuples
iex> {:ok, value} = {:ok, "Successful!"}
{:ok, "Successful!"}
iex> value
"Successful!"
iex> {:ok, value} = {:error}
** (MatchError) no match of right hand side value: {:error}
```

## தொங்கவிடும் செயல்பாடு

பொருத்தும் செயல்பாட்டின் இடப்புறம் ஒரு மாறி இருக்குமெனில், வலப்புறமுள்ள மதிப்பு அம்மாறிக்கு வழங்கப்படுகிறது. சிலசமயங்களில் இச்செயல்பாடு தேவையற்றது. அச்சமயங்களில் நாம் `^` என்று குறிக்கப்படும் தொங்கவிடும் செயல்பாட்டைப்பயன்படுத்தலாம்.

ஒரு மாறியைத்தொங்கவிடும்போது, அதன் மதிப்பை பாங்குபொருத்துவதற்கு எடுத்துக்கொள்கிறோம். ஒரு எடுத்துக்காட்டைப்பார்க்கலாம்:

```elixir
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
iex> {x, ^x} = {2, 1}
{2, 1}
iex> x
2
```

எலிக்சரின் 1.2 பதிப்பில், மேப்புகளின் திறவுச்சொற்களையும், செயற்கூற்றின் உருபுகளையும் கூட தொங்கவிடமுடியும்:

```elixir
iex> key = "hello"
"hello"
iex> %{^key => value} = %{"hello" => "world"}
%{"hello" => "world"}
iex> value
"world"
iex> %{^key => value} = %{:hello => "world"}
** (MatchError) no match of right hand side value: %{hello: "world"}
```

செயற்கூற்றின் உருபுகளைத்தொங்கவிடுவதற்கான எடுத்துக்காட்டு:

```elixir
iex> greeting = "Hello"
"Hello"
iex> greet = fn
...>   (^greeting, name) -> "Hi #{name}"
...>   (greeting, name) -> "#{greeting}, #{name}"
...> end
#Function<12.54118792/2 in :erl_eval.expr/5>
iex> greet.("Hello", "Sean")
"Hi Sean"
iex> greet.("Mornin'", "Sean")
"Mornin', Sean"
```