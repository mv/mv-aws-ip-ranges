# vim:ft=ruby:

require 'json'

###
### from json to text output
###
def ranges
  res = []
  var = JSON.parse( %x{ cat ip-ranges.json } )
  var['prefixes'].each { |p| res.push sprintf "%-15s  %-20s  %s", p['region'], p['service'], p['ip_prefix'] }
  res.sort
end

###
### about ip-ranges.json
###
desc "json: get latest json"
task :'get-json' do
  sh 'wget https://ip-ranges.amazonaws.com/ip-ranges.json'
end

desc "json: get the creation date"
task :created do
  sh %Q{ cat ip-ranges.json | jq '.createDate' }
end

###
### utilities
###
desc "json: all regions"
task :regions do
  sh %Q{ awk -F: '/region/ {print $2}' ip-ranges.json | sort | uniq | sed -e 's/[",]//g'}
end

desc "json: all services"
task :services do
  sh %Q{ awk -F: '/service/ {print $2}' ip-ranges.json | sort | uniq | sed -e 's/[",]//g'}
end

###
### real-deal
###
desc "ranges: rake ranges [r=sa-east-1] [s=ec2]"
task :ranges do
  # multiple filters
  res = ranges
  if ENV['r']
    res = res.grep( /#{ENV['r']}/i )
  end
  if ENV['s']
    res = res.grep( /#{ENV['s']}/i )
  end
  puts res
end

task :default do
  sh 'rake --tasks'
end

