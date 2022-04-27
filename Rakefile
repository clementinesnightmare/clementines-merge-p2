require 'bundler'
Bundler.setup

require 'fileutils'
require 'json'

COLLECTIONS = {
  'Aclipse' => 1,
  'Alyssa' => 500,
  'Axel' => 1,
  'Churchill' => 500,
  'Culus' => 500,
  'Dante' => 500,
  'Duplicity' => 1,
  'Farrel' => 1,
  'Fear Collector' => 500,
  'Greef' => 500,
  'Grole' => 1,
  'Herald' => 500,
  'Hiroh' => 500,
  'Jett' => 1,
  'Kitchbane' => 1,
  'Kohrg' => 500,
  'Kryss' => 1,
  'Lizoreth' => 500,
  'Maxx Knight' => 500,
  'MCD' => 1,
  'Prince' => 500,
  'Rayne' => 500,
  'Ricci' => 500,
  'Roland' => 500,
  'Rosaria' => 500,
  'Roux Atome' => 500,
  'Sasha Knight' => 500,
  'Scavage' => 1,
  'Shivv' => 500,
  'Skorch' => 500
}

COLLECTION_DIR = "collection"
IMG_DIR = "images"
META_DIR = "json"

namespace :collection do
  desc "generate collection metadata"
  task :generate_metadata do
    collectionMetaDir = File.join(COLLECTION_DIR, META_DIR)
    output = []
    10010.times do |i|
      j = JSON.parse(File.read(File.join(COLLECTION_DIR, META_DIR, "#{i}.json")))
      output.push(j)
    end
    File.write(File.join(COLLECTION_DIR, META_DIR, "_metadata.json"), JSON.pretty_generate(output))
  end

  desc "shuffle collection"
  task :shuffle do
    # Shuffling code.
    puts "Already shuffled."
    exit(0)

    Dir.mkdir(COLLECTION_DIR) unless File.exists?(COLLECTION_DIR)
    collectionImgDir = File.join(COLLECTION_DIR, IMG_DIR)
    Dir.mkdir(collectionImgDir) unless File.exists?(collectionImgDir)
    collectionMetaDir = File.join(COLLECTION_DIR, META_DIR)
    Dir.mkdir(collectionMetaDir) unless File.exists?(collectionMetaDir)

    grabs = []
    COLLECTIONS.each do |k, v|
      v.times do |i|
        grabs.push([k,i])
      end
    end
    grabs.shuffle!

    10010.times do |i|
      puts "Grabs left -> #{grabs.length}"
      randChar, randID = grabs.pop
      randJsonFile = File.join(randChar, META_DIR, "#{randID}.json")

      randJson = JSON.parse(File.read(randJsonFile))
      imageExt = randJson["image"].include?(".mp4") ? "mp4" : "png"
      associatedFile = randJsonFile.sub(META_DIR, IMG_DIR).sub("json", imageExt)

      puts randJsonFile
      puts associatedFile

      FileUtils.mv(associatedFile, File.join(collectionImgDir, "#{i}.#{imageExt}"))
      newJsonFile = File.join(collectionMetaDir, "#{i}.json")

      correctedJson = {}
      correctedJson["name"] = randJson["name"].sub(/\d+/, i.to_s)
      correctedJson["image"] = randJson["image"].sub(/\d+/, i.to_s)
      correctedJson["attributes"] = randJson["attributes"]

      correctedJson["attributes"].each_with_index do |obj, idx|
        if obj["trait_type"] == "Character"
          correctedJson["attributes"][idx]["trait_type"] = "Skin"
        end
      end

      correctedJson["attributes"].unshift({ "trait_type": "Character", "value": randChar })

      puts JSON.pretty_generate(correctedJson)

      File.write(newJsonFile, JSON.pretty_generate(correctedJson))
    end
  end

  desc "info"
  task :info do
    puts "Current Characters: \n" + COLLECTIONS.keys.join(", ") + "\n\n"
    puts "Character Count: " + COLLECTIONS.length.to_s
  end
end

task :default => 'collection:info'
