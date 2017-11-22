
A mod which resets all word progress when you mistype.





### Changes:

Added to TYPING_MANAGER.update() after this.OnMisType():
```
this.PotentialSwitchArray.Clear();
HashSet<INTERACTABLE_TEXT>.Enumerator enumeratora = this.ActiveTextArray.GetEnumerator();
while (enumeratora.MoveNext())
{
	if (enumeratora.Current == null)
	{
		this.RemoveActiveText(enumeratora.Current);
	}
	else
	{
		enumeratora.Current.ChangeRenderer(false);
		this.RemoveActiveText(enumeratora.Current);
	}
}

```
