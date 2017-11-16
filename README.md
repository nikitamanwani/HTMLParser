# HTML Parser

sub  trim { my $s = shift; $s =~ s/^\s+|\s+$//g; return $s };
print "Enter a filename to look up: ";
my $filename = <STDIN>; 
chomp $filename; 
exit 0 if ($filename eq ""); 

open(my $fh, '<:encoding(UTF-8)', $filename)
	or die "Could not open file '$filename' $!";
     
$html_data="";
while (my $row = <$fh>) {
   chomp $row;
   $html_data=$html_data." ".$row   
}
     
$html_data =~ s{ \s+}{ }gxms; 

print "Enter a tag to look up: ";
my $tag_name = <STDIN>; 
chomp $tag_name; 
exit 0 if ($tag_name eq ""); 

my $open_tag='<' . $tag_name;
my $close_tag='</' . $tag_name . '>';
print "Open tag :",$open_tag, "\n";
print "Close tag :",$close_tag, "\n";

my @words = split($close_tag, $html_data);

my $occurences = () = $html_data =~ /$open_tag/g;  

open my $fh, '>', 'output.txt';

my $count=0;
foreach my $val (@words) {
    my $index = index($val, $open_tag);
	if($index >= 0){
		$index=$index+length $open_tag;
		my $substring=substr $val, $index;
		my $substring=trim($substring);
		print {$fh} $substring . "\n";
		$count+=1;
		
	}
  }

print "my count is $count\n";
close $fh;
