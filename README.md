### GitLab Git

GitLab wrapper around git objects. Use patched Grit as main library for parsing git objects

#### Code status

* [![Build Status](https://travis-ci.org/gitlabhq/gitlab_git.png?branch=master)](https://travis-ci.org/gitlabhq/gitlab_git)
* [![Code Climate](https://codeclimate.com/github/gitlabhq/gitlab_git.png)](https://codeclimate.com/github/gitlabhq/gitlab_git)
* [![Coverage Status](https://coveralls.io/repos/gitlabhq/gitlab_git/badge.png?branch=master)](https://coveralls.io/r/gitlabhq/gitlab_git)


### How to use: 

#### Before

Set repositories storage:

    Gitlab::Git::Repository.repos_path = '/home/git/repositories'

#### Repository

    # Init repo with relative path according to repos_path. 
    # Example: 
    #  if 
    #    repos path is '/home/git/repos'
    #    full path is '/home/git/repos/namespace/project.git'
    #  then: 
    #    Gitlab::Git::Repository.new('namespace/project.git') 
    # 
    repo = Gitlab::Git::Repository.new('gitlab/gitlab-ci')

    repo.path_to_repo
    # "/home/git/repositories/gitlab/gitlab-ci.git"

    repo.commit
    # #<Gitlab::Git::Commit:0x00000009e0fde0>

    repo.commit("23adsa43")
    # #<Gitlab::Git::Commit:0x00000004a0fda3>

    # Get 10 recent commits for `master` branch for `app` directory
    repo.commits("master", 'app/', 10)

    # Get 10..15 recent commits for `master` branch
    repo.commits("master", nil, 10, 5)

    # Get 30..40 recent commits until `23dae49a`
    repo.commits("23dae49a", nil, 10, 5)

    # Get branches and tags
    repo.branches
    repo.tags

    # Get branch or tag names
    repo.branch_names
    repo.tag_names

    # Archive repo to `/tmp` dir
    repo.archive_repo('master', '/tmp')

    # Bare repo size in MB.
    repo.size
    # 10.43
    
    # Commits between branches
    repo.commits_between('dev', 'master')
    # [ <Gitlab::Git::Commit:0x000..>, <Gitlab::Git::Commit:0x000..>]
    
    # Diff between branches
    repo.diffs_between('dev', 'master')
    # [ <Gitlab::Git::Diff:0x000..>, <Gitlab::Git::Diff:0x000..>]

    # Search for code
    repo.search_files('rspec', 'master')
    # [ <Gitlab::Git::BlobSnippet:0x000..>, <Gitlab::Git::BlobSnippet:0x000..>]
   

#### Tree

    # Tree object for root dir
    tree = Gitlab::Git::Tree.new(repo, '893ade32')

    # Tree object for sub dir
    tree = Gitlab::Git::Tree.new(repo, '893ade32', 'master', 'app/models/')

    # Get readme for this directory if exists
    tree.readme

    # Get directories  
    tree.trees
    # [ <Gitlab::Git::Tree:0x000>, ...]

    # Get blobs  
    tree.blobs
    # [ <Gitlab::Git::Blob:0x000>, ...]

    # Get submodules
    tree.submodules
    # [ <Grit::Submodule:0x000>, ...]

    # Check if subdir   
    tree.up_dir?

#### Commit

     # Get commit items from Gitlab::Git::Repository
     # 
     commit = repo.commit('master')

     commit.id
     commit.sha
     # ba8812a2de5e5ea191da6930a8ee1965873286e3

     commit.short_id
     # ba8812a2de

     commit.message
     commit.safe_message
     # Fix bug 891

     commit.parent_id
     # ba8812a2de5e5ea191da6930a8ee1965873286e3

     commit.diffs
     # [ <Gitlab::Git::Diff:0x000..>, <Gitlab::Git::Diff:0x000..>]
     
     commit.created_at 
     commit.authored_date
     commit.committed_date
     # 2013-07-03 22:11:26 +0300

     commit.committer_name
     commit.author_name
     # John Smith
     
     commit.committer_email
     commit.author_email
     # jsmith@sample.com
